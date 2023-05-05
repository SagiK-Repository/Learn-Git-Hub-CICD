문서정보 : 2023.04.11.~ 작성, 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

<br>

# Learn-Git-Hub-CICD
Git Hub에서 CICD를 활용하는 방법을 익힌다.

### 목표
- [x] : 1. CI/CD?
- [x] : 2. Git Hub Action
- [ ] : 3. Git Hub Action 구성요소
- [x] : 4. CI/CD 구축 (1) - 기본적인 Message 출력
- [x] : 5. CI/CD 구축 (2) - Tag에 따라 Message 출력
- [ ] : 6. CI/CD 구축 (3) - Marster Brantch Push시 출력

### 제작자
[@SAgiKPJH](https://github.com/SAgiKPJH)

<br>

---

<br>

# 1. CI/CD?
- <img src="https://user-images.githubusercontent.com/66783849/231069040-c8d4b38e-c8a7-4db7-a2b0-55e5a3198539.png" width="300">  
- Continuous Integration/Continuous Delivery (지속적 통합/지속적 배포)
  - "CI"는 개발자를 위한 자동화 프로세스인 지속적인 통합(Continuous Integration)
  - "CD"는 지속적인 서비스 제공 (Continuous Delivery) 또는 지속적인 배포 (Continuous Deployment)
- 애플리케이션 개발 단계를 자동화하여 애플리케이션을 더욱 짧은 주기로 고객에게 제공하는 방법이다.
- 간단히 말해서, 특정한 시간에 통합, 빌드, 테스트, 배포등의 동작을 자동화하는 방법이다.

### 참고자료

- https://www.redhat.com/ko/topics/devops/what-is-ci-cd

<br><br><br>

# 2. Git Hub Action

- <img src="https://user-images.githubusercontent.com/66783849/231070276-58dbb733-7546-4456-86b7-ff98d6e0d56d.png" width="150">
- Git Hub Action을 통해 CI/CD를 구성한다.
- 빌드,테스트,배포를 자동화 시키는 파이프라인을 만들 수 있다.

### 참고자료
- https://velog.io/@sgwon1996/GitHub-Action%EC%9C%BC%EB%A1%9C-CICD-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0

<br><br><br>

# 3. Git Hub Action 구성요소



<br><br><br>

# 4. CI/CD 구축 (1) - 기본적인 Message 출력

- GitHub에서 이미 제작한 Repository -> Actions를 접속한다.  
  <img src="https://user-images.githubusercontent.com/66783849/231161193-e5354ee6-8f61-493b-aa48-d3cf2c97726e.png">  
- Simple workflow를 선택한다.  
  <img src="https://user-images.githubusercontent.com/66783849/231161416-c53254dc-ca27-408a-81a9-e14112f38f39.png">  
- 자동으로 생성되는 blank.yml을 확인한다.  
  <img src="https://user-images.githubusercontent.com/66783849/231161818-cd63ca6f-ba63-4db9-81fa-78c1a8929fd3.png">  
- 이러한 yml 파일을 다음과 같이 수정한다. (파일 이름을 CICD-Test.yml로 설정하였다)  
  ```yml
  name: CI/CD-Test # workflow 이름
  
  # Controls when the workflow will run
  on:
    push: # push 이벤트가 발생하면
      branches: [ "main" ] # "main" 브랜치에서만 동작
  
  jobs:
    build: # job 이름
      runs-on: ubuntu-latest # 사용할 runner의 운영체제
      
      steps: # 실행할 작업 목록
        - uses: actions/checkout@v3 # v3 버전의 actions/checkout 를 사용
   
        - name: Run a one-line script # 작업 이름
          run: echo Hello, world! # 실행할 명령어
  
        - name: Run a multi-line script # 작업 이름
          run: | # 실행할 명령어들
            echo Add other actions to build,
            echo test, and deploy your project.
  ```
- 작성 후 commit한다.
- 이후 Action에 접속하여 실행여부를 확인한다.  
  <img src="https://user-images.githubusercontent.com/66783849/231165951-ec169923-5797-4ef1-8cb3-56816225e420.png">  
- 이후 다른 내용을 Commit 한 후 상황을 재 확인한다.

<br><br>

# 5. CI/CD 구축 (2) - Tag에 따라 Message 출력

- 1. GitHub Repository `.github/workflow` 폴더 내부에 존재하는 yml을 통해 Actions가 동작한다.
- 2. yml 파일을 만든다.
  ```yml
  on:
    push: # push 이벤트 감지
      tags: # 태그 이벤트 중
        - 'release*' # release 태그가 붙은 커밋 감지
  
  jobs:
    build: # build 작업
      runs-on: ubuntu-latest # 우분투 환경에서 실행
      
      steps: # 실행할 작업들
      
        - name: Checkout code # 코드 체크아웃 액션 실행
          uses: actions/checkout@v2
          
        - name: Run custom command # 사용자 정의 명령어 실행
          if: contains(github.ref, 'refs/tags/release') # github.ref에 'refs/tags/release' 문자열이 포함되어 있는지 확인
          
          run: | # 실행할 명령어들
            echo Add other actions to build,
            echo test, and deploy your project.
  ```
- 3. GitHub에서 Tag를 생성한다.  
  <img src="https://user-images.githubusercontent.com/66783849/236379090-5700d891-364b-41c3-b948-80cf6bd95b6f.png"/>  
  <img src="https://user-images.githubusercontent.com/66783849/236379064-0d42e8f2-0644-4191-a822-1e5aabe639c9.png"/>  
  - 문자가 있는 Tag를 생성하려면, git 또는 다른 플랫폼을 활용한다.  
  <img src="https://user-images.githubusercontent.com/66783849/236380160-eb513712-b79a-4abd-9021-4989ecacfdf3.png"/>  
- 4. 결과를 확인한다.  
  <img src="https://user-images.githubusercontent.com/66783849/236380152-c3382c41-490b-48c6-b660-7e348ddd7deb.png"/>  
  
<br><br>

# 6. CI/CD 구축 (3) - Marster Brantch Push시 출력

- 특정 Brantch에 push할 때 Actions가 동작하도록 한다.  
  
