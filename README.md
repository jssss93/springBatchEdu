# springBatchEdu
SpringBatch 교육용자료


# 개요

## 배치 어플리케이션이란?

## 배치 사전적 의미

## 배치 언제 사용해야하나?

1) 일정 주기로 실행해야 할 때
2) 실시간 처리가 어려운 대량의 데이터를 처리해야 할 때

## 배치 왜 써야 하나?

1. 구조화
2. 가독성
3. 유지보수 용이
4. 메타데이터를 활용하여 다양한 기능 제공 가능

## 스프링 배치의 핵심 도메인 이해 및 활용

스프링 배치에서 Job을 구성하는데 있어서 사용되는 여러 도메인들이 존재

Job, Step, Flow, Tasklet, JobInstance, Jobexecution, StepExecution, 등.. 이러한 도메인들에 대한 개념부터 확실하게 이해를 해야 올바른 Job을 구성하고 활용할 수 있다.

각 도메인들의 용어적 개념과 도메인들간의 관계를 이해함으로써 간단한 Job부터 복잡한 Job까지 원하는 Job을 체계적으로 구성하는 방법을 숙지

![img](https://cdn.inflearn.com/public/files/courses/327744/a1eeb5d6-ad49-457b-970d-0cf88ee90976/%EB%8F%84%EB%A9%94%EC%9D%B8%20%EC%9D%B4%ED%95%B4.PNG)





## JOB

![image-20220921231154561](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231154561.png)



## JOB Excution

![image-20220921231309735](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231309735.png)

status 실행상태

exit_code 실행 결과



## STEP



![image-20220921231522972](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231522972.png)

## StepExcution



![image-20220921231616925](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231616925.png)

## JobRepository, JobLauncher

![image-20220921231719931](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231719931.png)

## MetaData

/org/springframework/batch/core/schema-*.sql

스프링 배치에선 실행 및 관리 목적으로 Job, Step, JobParameters등 여러 도메인들의 정보를 관리(CRUD)할 수 있는 스키마를 제공한다. inMemory가 아닌 DB와 연동할 경우 메타 테이블이 생성되어야 하는데, 이러한 메타테이블에 대한 DB 스키마는 스프링 배치 라이브러리에 존재하며 DB유형별로 제공되니 사용하는 DB에는 적절한 sql을 확인하면 된다. 

물론, 직접 sql 쿼리를 가져다가 수동으로 생성을 해도 되고, 자동으로 생성할 수도 있다. 

![img](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd963727f-102a-4e51-89ea-affc96c7390f%2FUntitled.png&blockId=9d09ba0c-3b8c-4400-b2c1-33beb594ffae)

#### 수동 생성 - 쿼리 복사 후 직접 실행

#### 자동 생성 - 프로퍼티 에 자동 생성 속성 지정

spring: 

​	batch:

​		jdbc:		

​		initialize-schema: always(or EMBEDDED, NEVER)

YAML

여기서 속성은 ALWAYS, EMBEDDED, NEVER 세 가지를 선택할 수 있는데 각각 다음과 같은 속성을 가진다. 

•ALWAYS: 스크립트는 항상 실행되고 RDBMS설정이 되어 있을 경우 내장  DB보다 우선 실행한다.

•EMBEDDED: 내장 DB일 때만 실행되며 스키마가 자동생성된다, (DEFAULT)

•NEVER: 스크립트 항상 실행안하고 내장 DB일 경우 스크립트 생성이 되지 않아 오류가 발생한다. 운영에서는 수동으로 스크립트 생성 후 설정하는 것이 권장된다.

[![img](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F94b91f5c-a9c9-4e49-bc43-2a883c796900%2FUntitled.png&blockId=c0d0f442-6d64-489a-ae6a-3ac39f63ae1b)](https://docs.spring.io/spring-batch/docs/3.0.x/reference/html/metaDataSchema.html)

#### Job 관련 테이블

•BATCH_JOB_INSTANCE

: Job이 실행될 때 JobInstnace 정보가 저장되며 Job_name과 job_key로 하여 하나의 데이터가 저장된다.

•BATCH_JOB_EXECUTION

: Job의 실행정보가 저장되며 Job 생성, 시작, 종료 시간, 실행 상태, 메세지 등을 관리한다.

•BATCH_JOB_EXECUTION_PARAMS

: Job과 함께 실행되는 JobParameter 정보를 저장한다. 

•ATCH_JOB_EXECUTION_CONTEXT

: Job의 실행 동안 여러가지 상태정보나 공유 데이터를 JSON형태로 직렬화 해서저장한다.

(중요) Step간 서로 공유가 가능하다.

#### Step 관련 테이블

•BATCH_STEP_EXECUTION

: Step의 실행정보가 저장되며 생성,시작,종료시간, 실행상태, 메시지 등을 관리한다.

•BATCH_STEP_EXECUTION_CONTEXT

: Step의 실행동안 여러가지 상태정보와 공유 데이터를 JSON형태로 직렬화 하여 저장한다.

Step별로 저장되며 Step간에 서로 공유할 수 없다.

### ![img](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) 메타 테이블 더 자세히 살펴보기

mysql 을 기준으로 설명한다.

#### 1. BATCH_JOB_INSTANCE

CREATE TABLE BATCH_JOB_INSTANCE  ( JOB_INSTANCE_ID BIGINT  NOT NULL PRIMARY KEY , VERSION BIGINT , JOB_NAME VARCHAR(100) NOT NULL, JOB_KEY VARCHAR(32) NOT NULL, constraint JOB_INST_UN unique (JOB_NAME, JOB_KEY) ) ENGINE=InnoDB;

SQL

•JOB_INSTANCE_ID : 고유하게 식별할 수 있는 기본 키

•VERSION: 업데이트 될 때마다 1씩 증가하는 필드

•JOB_NAME: Job을 구성할 때 부여하는 Job의 이름

•JOB_KEY: job_name과 jobParameter를 합쳐 해싱한 해시코드를 저장하는 필드

#### 2. BATCH_JOB_EXECUTION

CREATE TABLE BATCH_JOB_EXECUTION  ( JOB_EXECUTION_ID BIGINT  NOT NULL PRIMARY KEY , VERSION BIGINT  , JOB_INSTANCE_ID BIGINT NOT NULL, CREATE_TIME DATETIME(6) NOT NULL, START_TIME DATETIME(6) DEFAULT NULL , END_TIME DATETIME(6) DEFAULT NULL , STATUS VARCHAR(10) , EXIT_CODE VARCHAR(2500) , EXIT_MESSAGE VARCHAR(2500) , LAST_UPDATED DATETIME(6), JOB_CONFIGURATION_LOCATION VARCHAR(2500) NULL, constraint JOB_INST_EXEC_FK foreign key (JOB_INSTANCE_ID) references BATCH_JOB_INSTANCE(JOB_INSTANCE_ID) ) ENGINE=InnoDB;

SQL

•JOB_EXECUTION_ID : jobExecution을 고유하게 식별할 수 있는 기본 키로 JOB_INSTANCE와 1:N

•VERSION : 업데이트 될 때마다 1씩 증가하는 필드

•JOB_INSTANCE_ID: JOB_INSTANCE의 키

•CREATE_TIME: 실행(Execution)이 생성된 시점

•START_TIME: 실행(Execution)이 시작된 시점

•END_TIME : 실행(Execution)이 종료된 시점 ( job실행 도중 오류가 발생해 중단되면 값이 저장되지 않을 수도 있다. 

•STATUS: 실행 상태(BatchStatus)를 저장한다.(ex: COMPLETED, FAILED, STOPPED, STARTED, …)

•EXIT_CODE: 실행 종료 코드(ExitStatus)를 저장한다.(ex. COMPLETED, FAILED, UNKNOWN, …)

•EXIT_MESSAGE: Status가 실패일 경우 실패 원인을 저장한다.

•LAST_UPDATED: 마지막 실행(Execution) 시점

#### 3. BATCH_JOB_EXECUTION_PARAMS

CREATE TABLE BATCH_JOB_EXECUTION_PARAMS  ( JOB_EXECUTION_ID BIGINT NOT NULL , TYPE_CD VARCHAR(6) NOT NULL , KEY_NAME VARCHAR(100) NOT NULL , STRING_VAL VARCHAR(250) , DATE_VAL DATETIME(6) DEFAULT NULL , LONG_VAL BIGINT , DOUBLE_VAL DOUBLE PRECISION , IDENTIFYING CHAR(1) NOT NULL , constraint JOB_EXEC_PARAMS_FK foreign key (JOB_EXECUTION_ID) references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID) ) ENGINE=InnoDB;

SQL

•JOB_EXECUTION_ID: JobExecution 식별 키로 JOB_EXECUTION과 1:N 관계

•TYPE_CD: String, Long, Date, Double 타입 정보

•KEY_NAME: 파라미터 키 값

•STRING_VAL: 파라미터 문자 값

•DATE_VAL: 파라미터 날짜 값

•LONG_VAL: 파라미터 Long 값

•DOUBLE_VAL: 파라미터 Double 값

•IDENTIFYING: 식별 여부(true, false)

#### 4. BATCH_JOB_EXECUTION_CONTEXT

CREATE TABLE BATCH_JOB_EXECUTION_CONTEXT  ( JOB_EXECUTION_ID BIGINT NOT NULL PRIMARY KEY, SHORT_CONTEXT VARCHAR(2500) NOT NULL, SERIALIZED_CONTEXT TEXT , constraint JOB_EXEC_CTX_FK foreign key (JOB_EXECUTION_ID) references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID) ) ENGINE=InnoDB;

SQL

•JOB_EXECUTION_ID: JobExecution 식별키로 JOB_EXECUTION마다 각 생성된다.

•SHORT_CONTEXT: JOB의 실행 상태 정보로 공유 데이터 등의 정보를 문자열로 저장한다.

•SERIALIZED_CONTEXT: 직렬화(Serialized) 된 전체 컨텍스트를 저장한다.

#### 5. BATCH_STEP_EXECUTION

CREATE TABLE BATCH_STEP_EXECUTION  ( STEP_EXECUTION_ID BIGINT  NOT NULL PRIMARY KEY , VERSION BIGINT NOT NULL, STEP_NAME VARCHAR(100) NOT NULL, JOB_EXECUTION_ID BIGINT NOT NULL, START_TIME DATETIME(6) NOT NULL , END_TIME DATETIME(6) DEFAULT NULL , STATUS VARCHAR(10) , COMMIT_COUNT BIGINT , READ_COUNT BIGINT , FILTER_COUNT BIGINT , WRITE_COUNT BIGINT , READ_SKIP_COUNT BIGINT , WRITE_SKIP_COUNT BIGINT , PROCESS_SKIP_COUNT BIGINT , ROLLBACK_COUNT BIGINT , EXIT_CODE VARCHAR(2500) , EXIT_MESSAGE VARCHAR(2500) , LAST_UPDATED DATETIME(6), constraint JOB_EXEC_STEP_FK foreign key (JOB_EXECUTION_ID) references BATCH_JOB_EXECUTION(JOB_EXECUTION_ID) ) ENGINE=InnoDB;

SQL

•STEP_EXECUTION_ID: Step의 실행 정보 기본 키

•VERSION: 업데이트 될 때마다 1씩 증가

•STEP_NAME: Step을 구성할 때 부여하는 Step 이름

•JOB_EXECUTION_ID: JobExecution 기본키로 JobExecution과 1:N관계

•START_TIME: 실행(Execution)이 시작된 시점

•END_TIME: 실행(Execution)이 종료된 시점으로 Job 실행 도중 오류로 인해 중단된 경우 값이 저장되지 않을 수 있다.

•STATUS: 실행 상태(BatchStatus)를 저장한다.(ex: COMPLETED, FAILED, STOPPED, STARTED, …)

•COMMIT_COUNT:  트랜잭션 당 커밋되는 수 

•READ_COUNT: 실행시점에 Read한 Item count

•FILTER_COUNT: 실행도중 필터링 된 item수 

•WRITE_COUNT: 실행도중 저장되고 커밋된 Item 수

•READ_SKIP_COUNT: 실행도중 Read가 건너뛰어진(Skip) Item 수

•WRITE_SKIP_COUNT: 실행도중 Write가 건너뛰어진(Skip) Item 수

•PROCESS_SKIP_COUNT: 실행도중 Process가 건너뛰어진(Skip)된 Item 수

•ROLLBACK_COUNT: 실행도중 Rollback이 일어난 수 

•EXIT_CODE: 실행 종료 코드(ExitStatus)를 저장한다.

•EXIT_MESSAGE: Status가 실패일 경우 실패 원인 내용을 저장한다.

•LAST_UPDATED: 마지막 실행(Execution)시점

#### 6. BATCH_STEP_EXECUTION

CREATE TABLE BATCH_STEP_EXECUTION_CONTEXT  ( STEP_EXECUTION_ID BIGINT NOT NULL PRIMARY KEY, SHORT_CONTEXT VARCHAR(2500) NOT NULL, SERIALIZED_CONTEXT TEXT , constraint STEP_EXEC_CTX_FK foreign key (STEP_EXECUTION_ID) references BATCH_STEP_EXECUTION(STEP_EXECUTION_ID) ) ENGINE=InnoDB;

SQL

•STEP_EXECUTION_ID: StepExecution 식별 키, STEP_EXECUTION 마다 각 생성된다.

•SHORT_CONTEXT:  STEP의 실해 ㅇ상태 정보, 공유 데이터 등의 정보를 문자열로 저장한다.

•SERIALIZED_CONTEXT: 직렬화(Serialized)된 전체 컨텍스트









## ItemRader,Writer,Processor

![image-20220921231810190](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231810190.png)

## 비지니스 로직에 중점을둠



![image-20220921231945748](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921231945748.png)



## Job의 전체적인 구조

![image-20220921232017770](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232017770.png)



## Chunk-oriented Processing



![image-20220921232101958](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232101958.png)



## Chunk-oriented Processing(소스로이해하기)



![image-20220921232144134](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232144134.png)



## chunk 사이즈 관련



![image-20220921232243262](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232243262.png)

이유는 ~~





![image-20220921232405586](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232405586.png)



## 스프링 배치의 청크 기반 프로세싱 정복

스프링 배치에서 가장 핵심적인 기능 중에 하나가 바로 청크 기반 프로세싱이다. Chunk 개념을 도입하여 대용량의 데이터를 고성능으로 처리를 할 수 있도록 한다. 여기에 사용되는 API가 ItemReader, ItemProcessor, ItemWriter 이며, 청크 기반 프로세싱의 기본적인 개념과 원리를 학습하고 내부 아키텍처까지 파악한다.

![img](https://cdn.inflearn.com/public/files/courses/327744/f1058404-5dff-4689-9d92-e01da750445f/%EC%B2%AD%ED%81%AC.PNG)



## ItemProcessor

![image-20220921232439190](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232439190.png)

## Flow



![image-20220921232531802](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232531802.png)

![image-20220921232545200](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232545200.png)

![image-20220921232827331](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921232827331.png)



## 개발팁?



![image-20220921233108920](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921233108920.png)





![image-20220921233823982](C:\Users\CJS\AppData\Roaming\Typora\typora-user-images\image-20220921233823982.png)



# 스프링 배치의 오류 제어

배치 실행에 있어서 오류나 예외는 언제든지 발생할 가능성이 있습니다. 이러한 상황에서 오류로 인한 장애를 미리 예상하고 대비함으로써 배치 서비스가 완전히 중단되는 것이 아닌 일시적인 중단 혹은 예외를 무시하고 다음 단계로 가는 등의 처리를 함으로써 내결함성을 가진 배치 어플리케이션을 어떻게 구성할 수 있는지 학습하게 됩니다. 이와 관련된 기술인 Skip과 Retry 기능에 대한 자세한 내용과 실습을 진행합니다.

![img](https://cdn.inflearn.com/public/files/courses/327744/412a6349-f62c-4073-ab1a-2a47bb5d42aa/%EC%98%A4%EB%A5%98.PNG)





# 스프링 배치의 멀티 스레드 프로세싱 정복

대용량의 데이터 처리와 시간이 많이 소요되는 배치 처리는 단일 스레드가 아닌 멀티 스레드로 구성하여 동시에 병렬적인 배치 처리를 함으로써 더욱 효율적인 배치 처리가 이루어지도록 한다. 자바의 스레드 모델에 대한 기본적인 개념과 스프링 배치에서 제공하는 멀티 스레드 관련된 기술들을 먼저 이해하고 여러 멀티 스레드 유형의 배치처리 기술들을 익히게 됩니다.

![img](https://cdn.inflearn.com/public/files/courses/327744/8acd7900-7cf0-4b0d-b08a-bb48718bb3df/%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%93%9C.PNG)





# 각종Listener,



출처 : 



https://catsbi.oopy.io/616d4f42-9005-40e3-a0eb-c6bbe88da99a



https://www.youtube.com/watch?v=1xJU8HfBREY



https://www.inflearn.com/users/@leaven
