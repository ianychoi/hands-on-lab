# 0.1 ARM templates로 Resource 배포하기
Azure를 사용하는데 있어 리소스를 배포하는 많은 방법이 있습니다. 그 중 Templates을 이용하여 특정 환경을 배포할 수 있는 방법이 있습니다. 그 방법이 **Azure Resource Manager Templates(ARM Templates)** 이며, 이를 활용하면 특정 환경을 배포하는데 용이합니다.

여기서 우리는 앞으로 해야 할 실습을 위한 기초 설정을 ARM Template으로 배포하게 될 것이며, 보다 빠른 실습 환경 구성을 하겠습니다. 

ARM Template 배포는 당연하겠지만 Azure CLI 2.0 또는 Azure PowerShell이 필요합니다. 하지만 우리는 설치하는 시간을 줄이기 위해 Azure Cloud Shell을 이용하여 Azure CLI 2.0을 실행하겠습니다.

## Azure cloud Shell 설정
1. [Azure 웹 콘솔](https://portal.azure.com)에 접속합니다.

2. 오른쪽 위에 **>_** 아이콘을 클릭합니다.
![Azure Cloud Shell Icon](./image/0.1_Azure_Cloud_Shell_Icon.png)

3. 아래쪽에 다음 그림과 같이 **Azure Cloud Shell 시작** 화면이 나오면 **Bash(Linux)** 를 클릭합니다.
![Azure Cloud Shell init](./image/0.1_Azure_Cloud_Shell_init.png)

4. **탑재된 저장소가 없음** 화면이 나오면 구독을 선택한 후 **저장소 만들기** 버튼을 클릭합니다.
![none storage](./image/0.1_none_storage.png)

5. 저장소가 생성되고 Bash Shell이 접근될 때 까지 기다린 후 Shell이 뜨면 다음 명령어를 입력하여 Azure 구독 목록을 확인합니다.
    ```azure cli
    $ az account list
    ```
    > [!메모]
    >
    > JSON형식으로 출력된 값이 두 개 이상인 경우 로그인 한 계정에 연결된 구독(subscription)이 두 개 이상인 경우 입니다. 이 때 아래와 같은 명령어를 사용하여 특정 구독을 사용하겠다는 정의를 별도로 해 주어야 합니다.
    > ```Azurecli
    > az account set --subscription "<Subscription ID>"
    > ```

## ARM templates로 Resource 배포
6. 다음 명령어를 사용하여 ARM template인 *azure_template.json* 파일과 속성 값이 정의되어 있는 *parameters.json* 파일을 다운로드 받습니다.
    ```bash
    $ curl -O https://raw.githubusercontent.com/ianychoi/hands-on-lab/master/workshop-itpro-101/script/azure_template.json
    $ curl -O https://raw.githubusercontent.com/ianychoi/hands-on-lab/master/workshop-itpro-101/script/parameters.json
    ```

7. 다음 azure cli를 사용하여 리소스 그룹을 생성합니다.
    ```azure cli
    $ az group create -l <Azure 지역 이름> -n <리소스 그룹 이름>
    ```
    > [!메모]
    >
    > Azure 지역은 [이 링크](https://github.com/krazure/hands-on-lab/blob/master/SAL%201704%20IaaS%20%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0%20-%20Global%20Azure%20BootCamp%202017/1.1%20%EB%A6%AC%EC%86%8C%EC%8A%A4%20%EA%B7%B8%EB%A3%B9%20%EB%A7%8C%EB%93%A4%EA%B8%B0.md#azure-%EC%A7%80%EC%97%AD)를 참고하세요.

8. 리소스 그룹이 생성 완료되면 다음 명령어를 사용하여 ARM templates를 배포합니다.
    ```azure cli
    $ az group deployment create --name <배포 이름> --resource-group <리소스 그룹 이름> --template-file azure_template.json --parameters parameters.json
    ```
    > [!메모]
    >
    > 배포를 UI에서 확인해 보려면, 리소스 그룹에서 생성한 리소스 그룹을 클릭한 후 아래와 같이 배포 에 **실행 중** 을 클릭하여 확인할 수 있습니다.
    > ![0.1_deploy_check](./image/0.1_deploy_check.png)

9. 배포가 완료되면 Azure 웹 콘솔에서 리소스 그룹을 클릭하여 **리소스 그룹** 블레이드를 띄운 후 7번에서 생성한 리소스 그룹을 선택하여 배포된 리소스를 확인합니다.
