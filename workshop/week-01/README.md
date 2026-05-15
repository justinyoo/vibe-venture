# Week 01

## 개발 환경 설정

### 운영 체제별 개발 환경 설정

#### Windows

- Windows 11 Home 또는 Pro
- [Windows Terminal](https://aka.ms/terminal)
- [App Installer (WinGet)](https://apps.microsoft.com/detail/9nblggh4nns1)
- [PowerShell 7.6+](https://learn.microsoft.com/ko-kr/powershell/scripting/install/install-powershell-on-windows)

#### MacOS

- Sequoia 또는 Tahoe
- [Homebrew](https://brew.sh/)
- [iTerm2](https://iterm2.com/downloads.html)
- [zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

### 언어별 개발 환경 설정

- Python: [uv](https://docs.astral.sh/uv)
- JavaScript: [nvm (Windows)](https://github.com/coreybutler/nvm-windows), [nvm (MacOS)](https://github.com/nvm-sh/nvm)
- Java: [sdkman](https://sdkman.io)
- .NET: [.NET SDK](https://dotnet.microsoft.comdownload/dotnet/10.0)

### 개발 도구 설정

- [Microsoft Edge 브라우저](https://www.microsoft.com/edge/download)
- [VS Code](https://code.visualstudio.com/download)
- VS Code 익스텐션:
  - [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat)
  - [GitHub Copilot for Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azure-github-copilot)
  - [Web Search for Copilot](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-websearchforcopilot)
  - [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [GitHub CLI](https://cli.github.com/)
- [Azure CLI](https://aka.ms/azcli)
- [Azure Developer CLI](https://aka.ms/azd)
- [Docker Desktop](https://docs.docker.com/desktop)
- [GitHub Copilot 구독](https://github.com/settings/billing/licensing)
- [Azure 클라우드 구독](https://portal.azure.com/@innodgvibeventure.onmicrosoft.com)

### 서비스 로그인

- GitHub CLI 로그인: `gh auth login`
- GitHub Copilot CLI 로그인: `copilot login`
- Azure 클라우드 로그인
  - Azure CLI: `az login --tenant innodgvibeventure.onmicrosoft.com --use-device-code`
  - Azure Developer CLI: `azd auth login --tenant-id innodgvibeventure.onmicrosoft.com`

## GitHub Copilot 활용 간단한 앱 만들어보기

- Vibe Venture 리포지토리 클론하기
- GitHub Copilot 사용 준비:

  ```bash
  azd up
  ```

- VS Code GitHub Copilot 수동 등록

  ```bash
  echo "id: $(azd env get-value AZURE_OPENAI_GPT_DEPLOYMENT_NAME)"
  echo "name: Azure GPT 5.3 Codex"
  echo "url: $(azd env get-value AZURE_OPENAI_ENDPOINT)openai/responses?api-version=2025-04-01-preview"
  echo "apiKey: $(az cognitiveservices account keys list -g "rg-$(azd env get-value AZURE_ENV_NAME)" -n $(azd env get-value AZURE_OPENAI_NAME) --query key1 -o tsv)"
  ```

- GitHub Copilot CLI 수동 등록

  ```bash
  # Windows
  $env:COPILOT_PROVIDER_BASE_URL = "$(azd env get-value AZURE_OPENAI_ENDPOINT)deployments/$(azd env get-value AZURE_OPENAI_GPT_DEPLOYMENT_NAME)"
  $env:COPILOT_PROVIDER_TYPE = "azure"
  $env:COPILOT_PROVIDER_WIRE_API = "responses"
  $env:COPILOT_PROVIDER_API_KEY = "$(az cognitiveservices account keys list -g "rg-$(azd env get-value AZURE_ENV_NAME)" -n $(azd env get-value AZURE_OPENAI_NAME) --query key1 -o tsv)"
  $env:COPILOT_MODEL = "$(azd env get-value AZURE_OPENAI_GPT_DEPLOYMENT_NAME)"

  # MacOS
  export COPILOT_PROVIDER_BASE_URL="$(azd env get-value AZURE_OPENAI_ENDPOINT)deployments/$(azd env get-value AZURE_OPENAI_GPT_DEPLOYMENT_NAME)"
  export COPILOT_PROVIDER_TYPE="azure"
  export COPILOT_PROVIDER_WIRE_API="responses"
  export COPILOT_PROVIDER_API_KEY="$(az cognitiveservices account keys list -g "rg-$(azd env get-value AZURE_ENV_NAME)" -n $(azd env get-value AZURE_OPENAI_NAME) --query key1 -o tsv)"
  export COPILOT_MODEL="$(azd env get-value AZURE_OPENAI_GPT_DEPLOYMENT_NAME)"
  ```

- NEIS OpenAPI 키 등록: [https://open.neis.go.kr](https://open.neis.go.kr/portal/guide/actKeyPage.do)
  - 학교 정보 조회 API: [학교기본정보](https://open.neis.go.kr/portal/data/service/selectServicePage.do?page=1&rows=10&sortColumn=&sortDirection=&infId=OPEN17020190531110010104913&infSeq=1)
  - 학교별 급식 정보 조회 API: [급식식단정보](https://open.neis.go.kr/portal/data/service/selectServicePage.do?page=1&rows=10&sortColumn=&sortDirection=&infId=OPEN17320190722180924242823&infSeq=1)

- 학교별 급식 정보 조회 앱 - 콘솔 앱

## GitHub Copilot 활용 앱 설계하기

- 학교별 급식 정보 조회 앱 - 웹 앱
- 프롬프트 입력할 내용
  - 앱 개발 목적
  - 앱 작동 시나리오
  - 앱 개발에 필요한 기본 정보
  - 앱 구조
  - 앱 보안
