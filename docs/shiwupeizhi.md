
```xml
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>
```

```xml
<!-- 事务管理器 -->
<!-- <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean> -->
<!-- 事务属性 -->
<!-- <tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="list*" read-only="true"/>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice> -->
<!--  切面  -->
<!-- <aop:config>
    <aop:pointcut expression="execution(* com.weixin.service..*.*(..))" id="txPointcut"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config> -->
```

<!-- [示例](jar/Spring_Mybatis_Transation.zip ':ignore') -->