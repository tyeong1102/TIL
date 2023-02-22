# JPA 시작하기

## 프로젝트 생성

- Java 11을 사용하는 경우
    
    ```java
    <dependency>
        <groupId>javax.xml.bind</groupId>
        <artifactId>jaxb-api</artifactId>
        <version>2.3.0</version>
    </dependency>
    ```
    
     이 부분이 추가되지 않을 경우 main문의 실행 과정에서 EntityMangerFactory가 안되는 오류가 발생한다. 이러한 설정은 결국 스프링 부트가 알아서 해주기 때문에 지금만 문제가 된다.
    
- H2 데이터베이스
    
    persistence.xml 파일 안에 jdbc.url과 같은 값을 넣어주어야 한다.
    
    > `jdbc:h2:tcp://localhost/~/test`
    > 
    
    만약 test.mv.db가 기존에 있었다면, 완전히 삭제한 후 재설치 해야한다.
    

## 애플리케이션 개발

- JPA는 항상 EntityManagerFactory를 만들어야 한다.
    
    ```java
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    
    EntityManager em = emf.createEntityManager();
    
    EntityTransaction tx = em.getTransaction();
    ```
    
     EntityManagerFactory는 하나만 생성해서 데이터베이스 하나씩 할당되어 작동하고 애플리케이션 전체에 공유한다. 그리고 DB작업을 해야할 경우 반드시 EntityManager를 통해서 작업을 해야 한다. 이 때, JPA의 데이터의 변경이 있을 경우 반드시 Transaction안에서 일어나야 한다. 변경 후 데이터의 변경을 반영하기 위해서는 반드시 commit()을 해주어야 한다.
    
    괄호 안에 입력하는 것이 설정 파일의 설정 정보를 읽어오는 것이다. hello를 불러오는 이유도 설정 파일의 이름이 hello이기 때문이다.
    
    ```xml
    <persistence-unit name="hello">
        <properties>
            <!-- 필수 속성 -->
            <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/>
            <property name="javax.persistence.jdbc.user" value="sa"/>
            <property name="javax.persistence.jdbc.password" value=""/>
            <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/>
            <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/>
            <!-- 옵션 -->
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.use_sql_comments" value="true"/>
            <!--<property name="hibernate.hbm2ddl.auto" value="create" />-->
        </properties>
    </persistence-unit>
    ```