# 主要的疑问点(0119)

1. Eagle对应的机器学习模型是如何动态加载到Policy Engine中的?

2. Policy Engine和流计算引擎什么关系
 ![](/assets/PolicyEngine.png)
    - Metadata Manager是什么功能?
    - Dynamic Policy 如何实现?

3. Eagle API写的任务是如何转换成 Storm的 API的?
 ![](/assets/dagScheduler.png)

4. PolicyEngine 如何设计和实现?

5. Dynamic Policy 如何部署?

6. Some CEP Engine
    - Siddhi
    - Esper
    - Machine Learning

7. how to scalablity policyEvaluation
 ![](/assets/ScalabilityPolicyEvaluation.png)

8. light-weight metadata-driven store layer

9. 数据采集,如何收集yarn container中的日志信息

10. 主要的难点和痛点在哪?

11. 哪些地方可以优化?

12. 对ZK的依赖比较严重 
# 技术实现

## 框架结构
 ![](/assets/Architecture.png)

## 关键技术点
1. Persist ORM Framework
    - Nhibernate([link](http://example.com "HibernateORm.md"))
    - Castle ActiveRecord
    - EntityFramework
    - mybaits
    - Dapper
2. 
