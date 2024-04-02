# webpack启动流程
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655415447-21f15813-cabf-4025-b09f-b77f4f03011c.png#averageHue=%23ecd4ae&clientId=u8c6dfeb1-25a2-4&from=paste&id=u70742ab9&originHeight=1101&originWidth=1909&originalType=url&ratio=1&rotation=0&showTitle=false&size=954060&status=done&style=none&taskId=ua4bf2dd8-1120-4400-bc6b-5abab70c150&title=)

# webpack创建compiler
Compiler是webpack里面一个非常重要的的对象。它贯穿webpack生命周期的始终。他只有每次从新运行webpack的时候才会生成。
compilation是webpack编译代码的模板，主要存在于hook compile以及make之间。如果在watch模式下修改了源代码，会重新生成compilation。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655414865-2132df70-977f-4d06-87fb-7e1be676bca6.png#averageHue=%23f7f4f1&clientId=u8c6dfeb1-25a2-4&from=paste&id=uaf0967b4&originHeight=1263&originWidth=2291&originalType=url&ratio=1&rotation=0&showTitle=false&size=533279&status=done&style=none&taskId=u02a86792-eac8-47cb-8d75-f6814653a06&title=)
# compiler中run方法执行的hook
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655414503-16847229-48b1-417b-9c45-289c7dd5ec08.png#averageHue=%23f5f1ee&clientId=u8c6dfeb1-25a2-4&from=paste&id=u49289f9e&originHeight=1075&originWidth=1925&originalType=url&ratio=1&rotation=0&showTitle=false&size=298558&status=done&style=none&taskId=uc44d43c7-56c3-4873-b408-2be89ae6e67&title=)
# compilation中对module的处理
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655416038-d78f470d-ed02-4366-ae96-b3824e872491.png#averageHue=%23f3eeea&clientId=u8c6dfeb1-25a2-4&from=paste&id=u1fb0df6d&originHeight=1084&originWidth=1924&originalType=url&ratio=1&rotation=0&showTitle=false&size=458433&status=done&style=none&taskId=ue1e93492-f5dc-45ce-b998-0c22fb9f613&title=)
# module的build的阶段
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655417327-23dda5f4-0264-449d-bb1d-0b09bc9342c5.png#averageHue=%23f4f0ec&clientId=u8c6dfeb1-25a2-4&from=paste&id=u88f78c1c&originHeight=1077&originWidth=1925&originalType=url&ratio=1&rotation=0&showTitle=false&size=435872&status=done&style=none&taskId=u9ce98034-8eeb-41d1-b01d-a807e598266&title=)
# 输出asset阶段
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649655417710-65f00c97-1a8c-4b14-b810-8127620d98fa.png#averageHue=%23f3efeb&clientId=u8c6dfeb1-25a2-4&from=paste&id=u860a1750&originHeight=1086&originWidth=1924&originalType=url&ratio=1&rotation=0&showTitle=false&size=434419&status=done&style=none&taskId=u80abf956-ea28-4dee-af54-3119efd6927&title=)
