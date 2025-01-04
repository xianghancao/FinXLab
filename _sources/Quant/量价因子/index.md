pairs

```python
# coding=utf-8
import numpy as np
from simlib.signal.base import Signal
from simlib.signal.lib import *
import scipy.stats as st

#1. 降低换手率, 尝试JB黄的方式

#2. 观察不同EMA ,MA周期, 回撤时间的区别, 产生几个不同周期的线性组合,

#3. 使用pairs的方式, 寻找基本面, 的反转. 例如, 不按照价格定义相关性, 而是

#4. 成长指标, pe_growth, yty_growth.

#5. 不只使用价格作为计算相关性的工具, 加入市值, PE等..

#6. 按照市值划分

#7.按照300的行业权重详细划分

#8. 按照一级行业划分, 加入权重中性
class pairs_0406(Signal):

    def prebatch(self):
        np.set_printoptions(threshold='nan')
        # 以下是参数配置:
        self.yesterday_weights_factor = 0.5 #昨日权重系数
        self.ret_days = 20    #累计收益日期
        self.ret_mod = 'EMA'    #累计收益计算方式
        self.corr_days = 30   #相关性判断天数
        self.corr_percent = 80    #天数相关性的百分比
        self.ret_dif_percent = 80    #套利取的百分比
        self.use_stock_pool = 4    # 1: 000905, 2: 000300, 3: 000016, 4: 全市场 ret_days = 30 art = 1.68 tov = 0.18
        
        if self.use_stock_pool == 1:
            self.ind_data = self.fund.ZZ500_WGT    # 000905
        if self.use_stock_pool == 2:
            self.ind_data = self.fund.HS300_WGT    # 000300
        if self.use_stock_pool == 3:
            self.ind_data = self.fund.SZ50_WGT    # 000016  测试时期必须从2009年6月开始
        if self.use_stock_pool == 4:
            self.ind_data = self.eod.ClosePrice    # 全市场
            
        self.c = self.eod.ClosePrice
        self.SZ = self.fund.CAPQ0_MKTCAP                      #市值
        self.pre_c = self.eod.PreClosePrice
        self.all_day_ret = (self.c-self.pre_c)/self.pre_c
        self.all_ma_day_ret = self.function_wrapper(self.ret_mod,self.all_day_ret,timeperiod=self.ret_days)

# 矩阵处理: m:输入矩阵, if_change_to_0_and_1: 新矩阵使用0与1或使用矩阵原始值
    def process_matrix_1(self,m,percentile,if_change_to_0_and_1):
        temp = np.triu(m)
        temp = temp*(np.eye(len(m))-1)*(-1)
        temp_percentile = np.percentile(temp[np.logical_and(~np.isnan(temp),temp<>0)],percentile)
        if if_change_to_0_and_1==0:
            ret = temp*(temp>temp_percentile)
        elif if_change_to_0_and_1==1:
            ret = 1*(temp>temp_percentile)
        return ret
    
    def process_matrix_2(self,m,percentile,if_change_to_0_and_1):
        temp = np.triu(m)
        temp = temp*(np.eye(len(m))-1)*(-1)
        temp_percentile = np.percentile(temp[np.logical_and(~np.isnan(temp),temp<>0)],percentile)
        if if_change_to_0_and_1==0:
            ret = temp*(np.abs(temp)>temp_percentile)
        elif if_change_to_0_and_1==1:
            ret = np.sign(temp)*(np.abs(temp)>temp_percentile)
        return ret
        
    def generate(self,di):
        
        
    # 数据准备
        self.index_column_list = np.where(self.ind_data[di-1]>0)[0]
        index_column_ret = self.all_day_ret[di-self.corr_days:di,self.index_column_list]    # 计算相关性用
        index_column_ma_day_ret = self.all_ma_day_ret[di-1,self.index_column_list]    # 计算收益差用的N天累计ret的MA或EMA
        temp_ret = index_column_ma_day_ret.reshape(1,-1)    # 将一维的(500,)转换为二维的一行(1,500)
    # 相关性
        corr_matrix = np.corrcoef(index_column_ret.T)    # np.corrcoef将每行当一个元素计算,返回[行数,行数]形状的相关性矩阵
        corr_matrix_processed = self.process_matrix_1(corr_matrix,self.corr_percent,1)
    # 计算return差矩阵
        return_matrix = temp_ret.T - temp_ret    # 使用 列-行, 每个[i,j]位置的变量为, 第i-第j 的值
        return_matrix_processed = self.process_matrix_2(return_matrix,self.ret_dif_percent,0)
        
    # 两矩阵相乘,结果中提取累计权重加减值
        mix_matrix = return_matrix_processed * corr_matrix_processed
        weights_adj = np.nansum(mix_matrix,0)-np.nansum(mix_matrix,1)    # 第x个元素 = x列的和(每个行-x) - x行的和(x-每个列)
        #print np.nansum(mix_matrix,0).shape    # 有时不是500,而是450,????
        
        wgt = np.zeros(self.weights[-1].shape[0])
        
        for ix,val in enumerate(weights_adj):
            wgt[self.index_column_list[ix]] =   val
        
        sz=self.SZ[di-1]
        szprt=np.nanpercentile(sz,10)
        wgt[sz>szprt]=0
        ret =  wgt + self.weights[-1]*self.yesterday_weights_factor
        
        
        
        ret[self.ind_data[di-1]==0] = 0
        return ret
        
        
        
signal = pairs_0406

```

