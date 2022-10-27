[DB 연결 시 기본 Spring Legacy Project 설정 순서] 
===
~ postgresql의 경우 ~ 
---
    1. STS 상에서 프로젝트 생성
    2. 프로젝트명 및 서버 주소명 입력 
            : 주소명은 다른 프로젝트와 중복되지 않게
    3. pom.xml 설정
            1) 자바 버전 11, 릴리즈 버전 5.2.22로 설정(다른 버전으로 작업할 경우 해당 버전으로 설정)
            2) dependencies 태그 내에 사용하고자 하는 버전의 postgresql 드라이버 정보 삽입
            
                <!-- https://mvnrepository.com/artifact/org.postgresql/postgresql -->
		            <dependency>
    		            <groupId>org.postgresql</groupId>
    		            <artifactId>postgresql</artifactId>
    		            <version>42.5.0</version>
		            </dependency>
            
            
            3) plugin 영역의 메이븐 플러그인 설정 변경
                : 위에서 설정한 자바 버전을 참조하도록 ${java-version} 입력

              
              <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>2.5.1</version>
                    <configuration>
                        <source>${java-version}</source>                                
                        <target>${java-version}</target>                                    
                        <compilerArgument>-Xlint:all</compilerArgument>
                        <showWarnings>true</showWarnings>
                        <showDeprecation>true</showDeprecation>
                    </configuration>
               </plugin> 
               
               
        4. web.xml에 한글 변환 필터 추가
           
            <filter>
		        <filter-name>encodingFilter</filter-name>
		        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		        <init-param>
			        <param-name>encoding</param-name>
			        <param-value>UTF-8</param-value>
		        </init-param>
		        <init-param>
			        <param-name>forceEncoding</param-name>
			        <param-value>true</param-value>
		        </init-param>
	        </filter>
	        <filter-mapping>
		        <filter-name>encodingFilter</filter-name>
		        <url-pattern>/*</url-pattern>
	        </filter-mapping> 

        5. DB연결을 위한 클래스 작성
         : src\main\java 내 패키지 안에 작성(main 포함)

        6. DBMS에서 sql문 작성
            - 회원정보 테이블 및 컬럼 항목
               : 아이디, 비밀번호, 이름, 이메일, 생일, SNS, 가입일