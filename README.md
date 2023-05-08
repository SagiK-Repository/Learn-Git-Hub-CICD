문서정보 : 2023.04.11.~05.09. 작성, 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

<br>

# Learn-Git-Hub-CICD
Git Hub에서 CICD를 활용하는 방법을 익힌다.

### 목표
- [x] : 1. CI/CD?
- [x] : 2. Git Hub Action
- [x] : 3. Git Hub Action 구성요소
- [x] : 4. CI/CD 구축 (1) - 기본적인 Message 출력
- [x] : 5. CI/CD 구축 (2) - Tag에 따라 Message 출력
- [x] : 6. CI/CD 구축 (3) - Marster Brantch Push시 출력
- [x] : 7. CI/CD 구축 (4) - 자동 빌드에 대한 내용
- [x] : 8. CI/CD 구축 정리

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

- Event
  - Action을 실행할 이벤트 유형 및 트리거를 정의합니다.
- Job
  - Action을 실행하는 단위로, 하나 이상의 Step으로 구성됩니다.
- Step
  - Job에서 실행할 명령어와 작업을 정의합니다.
- Runner
  - Action이 실행될 환경을 정의합니다.
- Workflow
  - Event, Job, Step을 조합하여 작업의 흐름과 실행 순서를 정의합니다.

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
        - uses: actions/checkout@v3 # v3 버전의 actions/checkout 를 사용 (코드를 로컬 환경으로 거져온다)
   
        - name: Run a one-line script # 작업 이름
          run: echo Hello, world! # 실행할 명령어
  
        - name: Run a multi-line script # 작업 이름
          run: | # 실행할 명령어들
            echo Add other actions to build,
            echo test, and deploy your project.
  ```
- 처음 name을 정의하면  다음과 같이 이름이 등록이 된다. 그렇지 않으면 파일 이름이 등록된다.  
  <img src="https://user-images.githubusercontent.com/66783849/236629152-5cdd1199-413a-41b6-a162-5ced30ab2dd4.png"/>
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
      
        - name: Checkout code # 코드 체크아웃 액션 실행 (코드를 로컬 환경으로 거져온다)
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

- 특정 Brantch에 push할 때 Actions가 동작하도록 한다.(main Brantch)  
  ```yml
  on:
    push: # push 이벤트 감지
      pull_request: # 브랜치 이벤트 중
        - 'main*' # main로 시작하는 브랜치에 대한 pull request 이벤트 감지
  
  jobs:
    build: # build 작업
      runs-on: ubuntu-latest # 우분투 환경에서 실행
      steps: # 실행할 작업들

        - name: Checkout code # 코드 체크아웃 액션 실행 (코드를 로컬 환경으로 거져온다)
          uses: actions/checkout@v2

        - name: Run custom command # 사용자 정의 명령어 실행
          if: startsWith(github.ref, 'refs/heads/master') # github.ref가 'refs/heads/master'로 시작하는지 확인

          run: | # 실행할 명령어들
            echo Add other actions to build, # 빌드할 다른 작업 추가
            echo test, and deploy your project. # 프로젝트 테스트 및 배포
  ```
- Brantch를 만들어 main으로 push한다.  
  <img src="https://user-images.githubusercontent.com/66783849/236381564-19b4af27-0246-4c5d-854f-30e9aa5ca33a.png"/>  
  <img src="https://user-images.githubusercontent.com/66783849/236381906-d6a82a24-2d37-46e2-88f7-35676a7d137c.png"/> 

<br><br>

# 7. CI/CD 구축 (4) - 자동 배포

- 특정 Brantch에 Push가 되었을 때, 자동 배포와 더불어 특정 내용으로 배포할 수 있도록 한다.  
- Main 브랜치에 Merge할 때, "Release"라는 문자로 시작하는 내용으로 Commit 했을 경우에 같은 내용으로 Release를 구성할 수 있도록 한다.  
- Release를 구성하도록 하기 위해서는 GitHub Token이 필요하다. 이때 자동으로 제공되는 GITHUB_TOKEN를 활용하면 된다.
- 수동으로 GitHub Token 생성 방법
  - GitHub > 계정 Setting > Developer settings > Personal access tokens > Generate new token  
  <img src="https://user-images.githubusercontent.com/66783849/236632195-18ca4171-c606-4f6d-b76b-3e56b26cdc12.png"/>  
- GitHub 환경 변수 지정 방법
  - GitHub > 원하는 Repository > Setting > Secret and variables > Actions > New repository secret > "JJU_TOKEN" 생성후 Token 값을 입력 > Add secre
  - 이러한 Secret 환경 변수는 계정을 가지고 있지 않은 한은 접근이 불가능 하다.  
  - 다음과 같이 활용할 수 있다. `token: ${{ secrets.JJU_TOKEN }}`  
  <img src="https://user-images.githubusercontent.com/66783849/236632643-564094a4-c7cd-4ea1-bb74-f6bf7835244f.png"/>
- 다음과 같이 yml 파일을 구성한다.  
  ```yml
  name: Deploy to production # workflow 이름 지정
  
  on:
    push:
      branches:
        - 'main*' # main로 시작하는 브랜치에 대한 push 이벤트 감지
  
  jobs:
    build-and-deploy:
      runs-on: ubuntu-latest # 실행할 runner 환경 지정
      
      steps:
      - name: Checkout code
        uses: actions/checkout@v2
        with:
          ref: ${{ github.ref }}
        # 코드를 checkout
          
      - name: Build and test # 빌드 및 테스트 수행
        run: |
          # Add build and test steps here
          
      # 최근 commit의 메시지를 가져와서 message.txt 파일에 저장하고, output으로 message 변수에 저장
      - name: Merge commit message 
        run: |
          git log -1 --pretty=%B > message.txt
          echo "::set-output name=message::$(cat message.txt)"
        id: merge_message

      - name: Get Release Version
        if: startsWith(steps.merge_message.outputs.message, 'Release')
        id: extract_release_version # release 버전 추출
        run: |
          echo "::set-output name=version::$(echo ${{ steps.merge_message.outputs.message }} | sed 's/Release //')" # release 태그로부터 version 정보 추출하여 output으로 설정
        
      - name: Create release tag
        if: startsWith(steps.merge_message.outputs.message, 'Release') # startsWith 함수를 사용하여 message가 'Release'로 시작하는지 확인하고, tag 생성
        uses: actions/create-release@v1 
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ steps.extract_release_version.outputs.version }} # 반드시 v0.1.1와 같이 v 접두사가 들어가야 한다.
          release_name: Release v${{ github.run_number }}
          body: ${{ steps.merge_message.outputs.message }}
          draft: false
          prerelease: false
          
      - name: Deploy to production # 배포 단계 추가
        if: startsWith(steps.merge_message.outputs.message, 'Release')
        run: |
          # Add deployment steps here
  ```
- main Brantch로 "Release v0.1.3"Command을 시도한다.  
  <img src="https://user-images.githubusercontent.com/66783849/236631261-b016e750-66ec-4e4f-91ac-0b9e955b9c19.png"/>  
- Release 결과를 다음과 같이 확인할 수 있다.  
  <img src="https://user-images.githubusercontent.com/66783849/236633898-c665019d-6383-46c6-b40d-417ab668f584.png"/>  
  <img src="https://user-images.githubusercontent.com/66783849/236634010-383ea168-d1bd-4ef2-b904-6c97f9819539.png"/>

<br><br>

# 8. CI/CD 구축 (5) - 자동 빌드에 대한 내용

- ssh 그리고 linux 언어가 동작하는 환경에 대해서 프로젝트를 빌드할 수 있다면, 자동화 빌드도 가능할 것으로 추측이 된다.  
  ```yml
    - name: Build and test
      run: |
        # Add build and test steps here
        dotnet build MySolution.sln
        dotnet test MyTestsProject.csproj --logger "trx;LogFileName=test_results.xml"
  ```

<br><br>

# 9. CI/CD 구축 정리

- 이름 및 이벤트 정의
  ```yml
  name: Deploy to production # workflow 이름 지정
  
  on:
    push:
      branches:
        - 'main*' # main로 시작하는 브랜치에 대한 push 이벤트 감지
      branches: [ "main" ] # "main" 브랜치에서만 동작
      pull_request: # 브랜치 이벤트 중
        - 'main' # main 브랜치에 대한 pull request 이벤트 감지
      tags: # 태그 이벤트 중
        - 'release' # release 태그가 붙은 커밋 감지
  ```
- 환경 지정
  ```yml
  jobs:
    build: # build 작업
      runs-on: ubuntu-latest # 실행할 runner 환경 지정
    build-and-deploy:
      runs-on: ubuntu-latest # 우분투 환경에서 실행
  ```
- Git Checkout
  ```yml
      steps: # 실행할 작업들
        - name: Checkout code # 코드 체크아웃 액션 실행 (코드를 로컬 환경으로 거져온다)
          uses: actions/checkout@v2
    
        - uses: actions/checkout@v3 # v3 버전의 actions/checkout 를 사용 (코드를 로컬 환경으로 거져온다)

        - name: Checkout code
          uses: actions/checkout@v2
          with: # Action에 전달할 인자(argument)를 지정
            ref: ${{ github.ref }} # ref는 체크아웃할 브랜치를 지정 
            # ${{ github.ref }} 현재 이벤트에서 체크아웃해야 할 브랜치
  ``` 
- Build and test
  ```yml
        - name: Build and test
          run: |
            # Add build and test steps here
            dotnet build MySolution.sln
            dotnet test MyTestsProject.csproj --logger "trx;LogFileName=test_results.xml"
  ```
- 기타 활용
  ```yml
        - name: Run custom command # 사용자 정의 명령어 실행
          if: startsWith(github.ref, 'refs/heads/master') # github.ref가 'refs/heads/master'로 시작하는지 확인

          run: | # 실행할 명령어들
            echo Add other actions to build, # 빌드할 다른 작업 추가
            echo test, and deploy your project. # 프로젝트 테스트 및 배포

        - name: Run custom command # 사용자 정의 명령어 실행
          if: contains(github.ref, 'refs/tags/release') # github.ref에 'refs/tags/release' 문자열이 포함되어 있는지 확인
          
          run: | # 실행할 명령어들
            echo Add other actions to build,
            echo test, and deploy your project.
  ```
- Get Release
  ```yml
      - name: Get Release Version
        if: startsWith(steps.merge_message.outputs.message, 'Release')
        id: extract_release_version # release 버전 추출
        run: |
          echo "::set-output name=version::$(echo ${{ steps.merge_message.outputs.message }} | sed 's/Release //')" # release 태그로부터 version 정보 추출하여 output으로 설정
  ```
- release
  ```yml
      - name: Create release tag
        if: startsWith(steps.merge_message.outputs.message, 'Release') # startsWith 함수를 사용하여 message가 'Release'로 시작하는지 확인하고, tag 생성
        uses: actions/create-release@v1 
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ steps.extract_release_version.outputs.version }} # 반드시 v0.1.1와 같이 v 접두사가 들어가야 한다.
          release_name: Release v${{ github.run_number }}
          body: ${{ steps.merge_message.outputs.message }}
          draft: false
          prerelease: false
  ```
