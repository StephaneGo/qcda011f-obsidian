

Date: 16/06/2026
Tags: 
[[-Spring framework]]
[[-JPA]]

# Notes

Configuration de JPA dans une application Spring :

```yaml
spring:  
  application:  
    name: demoSpringJpa  
  
  #Connection to DB  
  datasource:  
    url: jdbc:sqlserver://localhost;databasename=DEMO_ENI_BD;integratedSecurity=false;encrypt=false;trustServerCertificate=false  
    username: sa  
    password: Pa$$w0rd  
  
  #Options to DB  
  jpa:  
    show-sql: true  
    properties:  
      hibernate:  
        format_sql: true  
    hibernate:  
      ddl-auto: create
```


# Références

