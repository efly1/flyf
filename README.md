
3. 使用示例
python
# 示例1: 从Yahoo Finance获取股票数据并分析
if __name__ == "__main__":
    # 创建分析器实例
    analyzer = EconomicAnalyzer()
    
    # 加载苹果公司(AAPL)的股票数据
    success = analyzer.load_data(
        source='api',
        ticker='AAPL',
        start_date='2020-01-01',
        end_date='2023-01-01'
    )
    
    if success:
        # 清洗数据
        analyzer.clean_data(methods=['drop_na', 'outliers'])
        
        # 计算基本统计量
        stats = analyzer.calculate_statistics()
        print("基本统计量:")
        print(stats['descriptive_stats'])
        
        # 计算经济指标
        indicators = analyzer.calculate_economic_indicators()
        print("\n经济指标:")
        for k, v in indicators.items():
            print(f"{k}: {v:.4f}")
        
        # 绘制时间序列图
        analyzer.plot_time_series(columns=['Open', 'High', 'Low', 'Close'])
        
        # 绘制收益率分布图
        returns = analyzer.processed_data['Close'].pct_change().dropna()
        analyzer.plot_distribution(returns.name, title="每日收益率分布")
        
        # 保存结果
        analyzer.save_results(stats, 'aapl_stats.csv')
        analyzer.save_results(indicators, 'aapl_indicators.xlsx', 'excel')
 
    # 示例2: 从本地文件加载数据
    # analyzer.load_data(source='file', file_path='economic_data.csv')
    # analyzer.clean_data()
    # analyzer.plot_time_series(columns=['GDP', 'Unemployment'])
