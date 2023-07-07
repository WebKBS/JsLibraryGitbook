---
description: 그래프 테두리 색상 변경 방법
---

# x축 y축 tick 색상 변경

```javascript
var myChart = new Chart(ctx, {
    type: 'bar',
    data: {
        // ...
    },
    options: {
        scales: {
            x: {
                ticks: {
                    color: 'red' // x 축(tick)의 색상
                }
            },
            
            y: {
                ticks: {
                    color: 'blue' // y 축(tick)의 색상
                }
            }
        }
    }
});
```
