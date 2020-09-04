﻿---
lab:
    title: '02 - Azure Policy'
    module: '모듈 01 – ID 및 액세스 관리'
---

# 랩 02: Azure Policy
# 학생용 랩 매뉴얼

## 랩 시나리오

Azure 정책의 사용 방법을 보여주는 개념 증명을 만들라는 요청을 받았습니다. 특히 다음이 필요합니다.

- 리소스가 특정 지역에서만 만들어지도록 허용하는 허용 위치 정책을 만듭니다.
- 허용 위치에서만 리소스가 만들어지는지 확인하기 위한 테스트

> 이 랩의 모든 리소스에 대해, **미국 동부** 지역을 사용하고 있습니다. 강사에게 이 영역이 수업에 사용할 영역인지 확인합니다. 

## 랩 목표

이 랩에서는 다음을 완료합니다.

- 연습 1: Azure Policy 구현. 

### 연습 1: Azure Policy 구현

#### 예상 시간: 20분

이 연습에서는 다음 작업을 수행합니다.

- 작업 1: Azure 리소스 그룹을 만듭니다. 
- 작업 2: 허용 위치 정책 할당을 만듭니다.
- 작업 3: 허용 위치 정책 할당이 작동하는지 확인합니다. 

#### 작업 1: 랩을 위한 리소스 그룹을 만듭니다. 

이 작업에서는 이 랩에 사용할 리소스 그룹을 만듭니다. 

1. Azure Portal **`https://portal.azure.com/`**에 로그인합니다.

    >**참고**: 이 랩에 사용 중인 Azure 구독에 Owner 또는 Contributor 역할이 있는 계정을 사용하여 Azure Portal에 로그인합니다.

1. Azure Portal 오른쪽 상단에 있는 첫 번째 아이콘을 클릭하여 Cloud Shell을 엽니다. 메시지가 표시되면 **PowerShell**을 선택하고 **스토리지를 만듭니다**.

1. Cloud Shell 창의 왼쪽 위 모서리에 있는 드롭다운 메뉴에서 **PowerShell**이 선택되었는지 확인합니다.

1. Cloud Shell 창의 PowerShell 세션에서 다음을 실행하여 리소스 그룹을 만듭니다(위치 매개 변수 값에 대해 강사와 함께 확인).

    ```powershell
    New-AzResourceGroup -Name AZ500LAB02 -Location 'East US'
    ```

1. 클라우드 셸 창 내의 PowerShell 세션에서 다음을 실행하여 리소스 그룹 목록이 나타나면 새 리소스 그룹이 만들어졌는지 확인합니다.

    ```powershell
    Get-AzResourceGroup | format-table
    ```

1. **Cloud Shell** 창을 닫습니다.

#### 작업 2: 허용 위치 정책 할당을 만듭니다.

이 작업에서는 허용 위치 정책 할당을 만들고 정책이 사용할 수 있는 Azure 지역을 지정합니다. 

1. Azure Portal에서 Azure Portal 페이지 상단의  **리소스, 서비스 및 문서 검색** 텍스트 상자에 **정책**을 입력하고 **Enter** 키를 누릅니다.

1. **정책** 블레이드의 **제작** 섹션에서 **정의**를 선택합니다.

1. 기본 제공된 여러 정의를 잠깐 검토하세요. **범주** 드롭다운 메뉴를 사용하여 정책 목록을 필터링합니다.

1. **검색** 텍스트 상자에 **허용 위치**를 입력합니다. 

   >**참고**: **허용 위치** 정책을 사용하면 리소스 그룹이 아닌 리소스 위치를 제한할 수 있습니다. 리소스 그룹의 위치를 제한하려면 **리소스 그룹에 대해 허용된 위치**정책을 사용할 수 있습니다. 

1. **허용 위치** 정책 정의를 클릭하여 세부 정보를 표시합니다. 

   >**참고**: 이 정책 정의는 위치 배열을 매개 변수로 사용합니다. 정책 규칙은 'if-then'문입니다. 'if' 절은 리소스 위치가 매개 변수 목록에 포함되었는지 확인하고 그렇지 않은 경우 'then' 절이 리소스 생성을 거부하거나 기존 리소스의 경우 이를 비준수로 표시합니다.

1. **허용 위치** 블레이드에서 **할당**을 클릭합니다.

1. **허용 위치** 블레이드의 **기본** 탭에서 **범위** 텍스트 상자 옆에 있는 줄임표(...) 버튼을  클릭하고 **범위**블레이드에서 다음 설정을 지정합니다.

   |설정|값|
   |---|---|
   |구독|Azure 구독명|
   |리소스 그룹| **AZ500LAB02** |

1. **선택**을 클릭합니다.

1. **허용 위치** 블레이드의 **기본** 탭에서 다음 설정을 지정합니다(기타 설정은 default 값을 유지합니다).

   |설정|값|
   |---|---|
   |할당 이름| **AZ500LAB02에 대해 영국 남부 허용** |
   |리소스 그룹| **AZ500LAB02** |
   |설명| **AZ500LAB02에 대해서만 영국 남부에서 리소스를 만들 수 있도록 허용** |
   |정책 적용| **사용** |

1. **다음**을 클릭합니다. 

1. **허용 위치** 블레이드에 있는 **매개 변수** 탭의 **허용 위치** 드롭다운 목록에서 **영국 남부**를 유일한 허용 위치로 선택합니다. 

   >**참고**: 하나 이상의 위치를 선택할 수 있습니다. 정책에 다른 매개 변수 집합이 필요한 경우 이 탭은 그러한 선택을 제공합니다. 

1. **검토 + 만들기**를 클릭한 다음 **만들기**를 클릭하여 정책 할당을 만듭니다. 

   >**참고**: 할당에 성공했으며 할당이 완료되는 데 약 30분이 걸릴 수 있다는 알림이 표시됩니다.

   >**참고**: Azure 정책 할당이 적용되는 데 최대 30분이 소요될 수 있는 이유는 전역으로 복제해야 하기 때문입니다. 일반적으로는 소요 시간이 몇 분 정도에 지나지 않습니다.  다음 작업이 실패하면 몇 분 동안 기다렸다가 다시 각 단계를 시도하십시오.

#### 작업 2 허용 위치 정책 할당을 테스트합니다.

이 작업에서는 허용 위치 정책 할당을 테스트합니다. 

1. Azure Portal 내 Azure Portal 페이지 상단의 **리소스, 서비스 및 문서 검색** 텍스트 상자에 **가상 네트워크**를 입력하고 **Enter** 키를 누릅니다.

1. **가상 네트워크** 블레이드에서 **+ 추가**를 클릭합니다.

   >**참고**: 먼저 미국 동부에서 가상 네트워크를 만들어 봅니다. 이 위치는 허용되는 위치가 아니므로 요청이 차단되어야 합니다. 

1. **가상 네트워크 만들기** 블레이드의 **기본** 탭에서 다음 설정을 지정합니다(기타 설정은 기본값을 유지합니다).

    |설정|값|
    |---|---|
    |리소스 그룹| **AZ500LAB02** |
    |이름| **myVnet** |
    |지역| **(US) 미국 동부** |

1. **검토 + 만들기**를 클릭합니다. 

1.  **가상 네트워크 만들기** 블레이드의  **검토 + 만들기** 탭에서

1. **유효성 검사 실패** 메시지의 메모 **만들기**를 클릭합니다.
1. 오류 메시지를 클릭하여 **오류** 블레이드를 엽니다.  리소스 **myVnet**의 배포를 정책에서 허용하지 않았다는 자세한 오류 메시지가 표시됩니다.

1. **오류** 블레이드를 닫고 **가상 네트워크 만들기** 블레이드에서 **기본** 탭을 클릭한 다음, **지역** 드롭다운 목록에서 **(유럽) 영국 남부**를 선택합니다.

1. **검토 + 만들기**를 클릭하여 유효성 검사가 통과되었는지 확인하고, **만들기**를 클릭하여 가상 네트워크가 성공적으로 만들어졌는지 확인합니다. 

> 연습 결과: 이 연습에서는 기본 제공 정책 정의를 선택하고 리소스 그룹에 할당하여 Azure 정책을 적용하는 방법을 배웠습니다.

**리소스 정리**

> 새로 만든 Azure 리소스 중에서 더 이상 사용하지 않는 리소스는 제거해야 합니다. 사용하지 않는 리소스를 제거하면 예기치 않은 비용이 발생하지 않습니다.

1. Azure Portal에서 Azure Portal의 오른쪽 상단의 첫 번째 아이콘을 클릭하여 Cloud Shell을 엽니다. 메시지가 표시되면 **다시 연결**을 클릭합니다.

1. Cloud Shell 창 내의 PowerShell 세션에서 다음을 실행하여 이 랩에서 만든 리소스 그룹을 제거합니다.
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB02" -Force -AsJob
    ```

1.  **Cloud Shell** 창을 닫습니다. 