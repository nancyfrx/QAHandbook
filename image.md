```mermaid
xychart-beta
    title "阶梯逼近法寻找最佳并发"
    x-axis [0, 1000, 2000, 3000, 4000]
    y-axis [0, 5, 10, 15]
    
    bar "错误率%" [0.1, 0.5, 2, 10, 25]
    line "响应时间s" [0.2, 0.5, 1.2, 3.5, 7.0]
        points [[1000,0.5], [2000,1.2], [3000,3.5], [4000,7.0]]
        
    annotation point [2000, 1.2]
        color green
        label "最佳并发区间"
```