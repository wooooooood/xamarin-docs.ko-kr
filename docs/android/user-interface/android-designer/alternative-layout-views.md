---
title: "대체 레이아웃 뷰"
description: "이 방법을 레이아웃 버전 관리가 가능 리소스 한정자를 사용 하 여 설명 합니다. 예를 들어 가로 모드로 장치가 있을 때만 사용 되는 레이아웃의 버전과 레이아웃 된 버전을 세로 모드에 대해서만 있을 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: c2df60a79ea3b5a0ff226cfaade0440db13fd5ea
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="alternative-layout-views"></a>대체 레이아웃 뷰

_이 방법을 레이아웃 버전 관리가 가능 리소스 한정자를 사용 하 여 설명 합니다. 예를 들어 가로 모드로 장치가 있을 때만 사용 되는 레이아웃의 버전과 레이아웃 된 버전을 세로 모드에 대해서만 있을 수 있습니다._


## <a name="creating-alternative-layouts"></a>대체 레이아웃 만들기

클릭는 **대체 레이아웃 보기** 아이콘 (왼쪽의 **장치**), 프로젝트에서 사용할 수 있는 대체 레이아웃을 나열 하는 미리 보기 창이 열립니다. 대체 레이아웃 없는 경우는 **기본** 뷰가 표시 됩니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![대체 레이아웃 보기 창](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "대체 레이아웃 보기 창")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![대체 레이아웃 보기 창](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

-----

옆에 녹색 더하기 기호를 클릭 하면 **새 버전**, **레이아웃 변형을 만드는** 이 레이아웃 변형에 대 한 리소스 한정자를 선택할 수 있도록 대화 상자가 열립니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![레이아웃 변형 만들기](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "레이아웃 변형 만들기")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![레이아웃 변형 만들기](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

-----


다음 예제에 대 한 리소스 한정자 **화면 방향을** 로 설정 된 **가로**, 및 **화면 크기** 로 변경 **큼**. 라는 새로운 레이아웃 버전을 만들면이 **큰 토지**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![큰 토지 변형](alternative-layout-views-images/vs/03-large-land-sml.png "큰 토지 변형")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![큰 토지 변형](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

-----


왼쪽의 미리 보기 창의 리소스 한정자 선택 항목의 효과 표시 되는지 확인 합니다. 클릭 하면 **추가** 대체 레이아웃을 만들고 해당 레이아웃에는 디자이너를 전환 합니다. **대체 레이아웃 보기** 미리 보기 창에 표시 다음 스크린샷에 표시 된 대로 레이아웃 작은 오른쪽 포인터를 통해 디자이너에 로드 됩니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![로드 된 레이아웃 표시기](alternative-layout-views-images/vs/04-new-layout-sml.png "로드 레이아웃 표시기")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![로드 된 레이아웃 표시기](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

-----



## <a name="editing-alternative-layouts"></a>대체 레이아웃 편집

대체 레이아웃을 만들 때 레이아웃의 모든 분기 버전에 적용 되는 단일 변경 하는 것이 좋습니다. 예를 들어 다음 모든 레이아웃의 노란색으로 단추 텍스트를 변경 하는 것이 좋습니다. 레이아웃 수가 많은 경우 번거롭고 오류가 발생 하기 쉬운 모든 버전에 단일 변경 내용을 전파 하는 경우 유지 관리 신속 하 게 될 수 있습니다. 

디자이너는 여러 레이아웃 버전의 유지 관리를 단순화 하려면 다음을 제공 합니다. 한 **다중 편집** 하나 이상의 레이아웃에서 변경 내용을 전파 하는 모드입니다. 둘 이상의 레이아웃을 사용할 수 없으면는 **다중 편집** 아이콘이 표시 됩니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![다중 편집 아이콘](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "다중 편집 아이콘")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![다중 편집 아이콘](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

-----


클릭는 **다중 편집** 아이콘을 사용할 수 있는 레이아웃 (아래와 같이)에 연결 되어 있음을 나타내는 선이 표시; 즉, 한 레이아웃을 변경한 경우 해당 변경 내용이 전파 되어 연결 된 모든 레이아웃 합니다. 다음 스크린샷에 표시 된 원형된 아이콘을 클릭 하 여 모든 레이아웃 연결을 끊을 수 있습니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![모든 레이아웃의 연결을 해제](alternative-layout-views-images/vs/06-multi-linked-sml.png "모든 레이아웃의 연결을 해제")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![모든 레이아웃의 연결을 해제합니다](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

-----


두 개 이상의 레이아웃을 설정한 경우 어떤 레이아웃 서로 연결을 확인 하려면 각 레이아웃 미리 보기의 왼쪽에 편집 단추를 선택적으로 전환할 수 있습니다. 예를 들어, 전파 하는 단일 변경 하려는 경우 첫 번째 및 세 가지 레이아웃 중 마지막 작업에 먼저 연결을 해제 하면 중간 레이아웃 다음과 같이 합니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![연결 끊기 중간 레이아웃](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "Unlink 중간 레이아웃")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![중간 레이아웃의 연결을 해제합니다](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)
 
-----
 

이 예제에서는 변경 중 하나로 **기본** 또는 **긴** 다른 레이아웃으로 제외한 레이아웃 전파 됩니다는 **큰 토지** 레이아웃 합니다. 



### <a name="multi-edit-example"></a>다중 편집 예제 

일반적으로 한 레이아웃을 변경 하면 다른 모든 연결 된 레이아웃에 동일한 변경 전파 됩니다. 예를 들어, 새 추가 `TextView` 위젯을 **기본** 레이아웃 및 텍스트를 변경 합니다. 문자열을 `Portrait` 모든 연결 된 레이아웃에 대해 동일한 변경이 발생 합니다. 여기에서 어떻게 보이는지는 **기본** 레이아웃: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TextView 추가](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView를 추가 합니다.")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![TextView를 추가 합니다.](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)
 
-----
 

`TextView` 에 추가 됩니다는 **큰 토지** 연결 되기 때문에 레이아웃 보기의 **기본** 레이아웃: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TextView를 가로로](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView를 가로 방향")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![가로 TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
-----
 

하지만 하나의 레이아웃에 로컬 변경 하려는 경우 (즉, 않으려면 다른 레이아웃에 전파 하 여 변경 내용을)? 이 수행 하려면 레이아웃을 수정 하기 전에 다음에 설명 된 대로 변경 하려면를 연결 해제 해야 합니다. 



### <a name="making-local-changes"></a>로컬 변경 

두 레이아웃 모두 추가 하려면 원하는 경우를 가정해 볼 `TextView`, 또한에 있는 텍스트 문자열을 변경 하려고 하지만 **큰 토지** 레이아웃을 `Landscape` 대신 `Portrait`합니다. 이 변경 하려면 만들면 **큰 토지** 두 레이아웃 모두 연결 된 동안 변경 됩니다 전파 되는 **기본** 레이아웃 합니다. 따라서 म 해야 먼저 연결을 끊을 두 개의 레이아웃 변경 하기 전의. 텍스트를 수정 했습니다 **큰 토지** 를 `Landscape`, Designer는 이러한 변경에 로컬 변경 임을 나타내기 위해 빨간색 프레임 표시는 **큰 토지** 레이아웃 이며 *하지* 다시 전파는 **기본** 레이아웃: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![로컬 변경](alternative-layout-views-images/vs/10-local-change-sml.png "로컬 변경")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![로컬 변경](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)
 
-----
 

클릭는 **기본** 볼 레이아웃의 `TextView` 텍스트 문자열은로 설정 되어 `Portrait`합니다. 



## <a name="handling-conflicts"></a>충돌 처리 

에 있는 텍스트의 색을 변경 하려는 경우는 **기본** 레이아웃 녹색을, 연결 된 레이아웃에 표시 되는 경고 아이콘이 표시 됩니다. 해당 레이아웃을 클릭 하 여 충돌을 표시 하기 위해 레이아웃을 열립니다. 충돌을 일으킨 위젯 빨간색 프레임으로 강조 표시 되 고 다음 메시지가 표시 됩니다: *최근 변경 내용으로 인해 충돌이 대체 레이아웃에서*합니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![충돌 변경](alternative-layout-views-images/vs/11-conflicting-change-sml.png "충돌 하는 변경")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![변경 내용이 충돌](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)
 
-----
 

A *충돌 상자* 충돌을 설명 하기 위해 위젯의 오른쪽에 표시 됩니다. 

[![충돌 경고](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

충돌 상자 변경 된 속성의 목록을 표시 하 고 해당 값을 나열 합니다. 클릭 하면 **무시 충돌** 속성 변경 하는이 위젯의에 적용 됩니다. 클릭 하면 **적용** 이 위젯의 연결 된 테이블에 해당 위젯 속성 변경을 적용할 **기본** 레이아웃 합니다. 모든 속성 변경이 적용 된 충돌은 자동으로 삭제 됩니다. 


### <a name="view-group-conflicts"></a>보기 그룹 충돌 

속성 변경 유일한 소스는 충돌 하지 않습니다. 삽입 / 위젯을 삭제할 때 충돌을 검색할 수 있습니다. 예를 들어,는 **큰 토지** 레이아웃에서 연결이 해제 됩니다는 **기본** 레이아웃 및 `TextView` 에 **큰 토지** 레이아웃을 끌어서 위에 `Button`, Designer는 충돌을 나타내기 위해 이동된 위젯을 표시:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![그룹 충돌 보기](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "그룹 충돌 보기")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![그룹 충돌 보기](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
-----
 

그러나는 표식이 없습니다.에 `Button`합니다. 하지만의 위치는 `Button` 변경 되었습니다는 `Button` 관련 없는 적용된 된 변경 내용을 표시는 **큰 토지** 구성 합니다. 

경우는 `CheckBox` 에 추가 되는 **기본** 레이아웃, 다른 충돌 생성 되 고 경고 아이콘이 표시 되는 **큰 토지** 레이아웃: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![확인란 충돌](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "확인란 충돌")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![확인란 충돌](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
-----
 

클릭 하는 **큰 토지** 레이아웃 충돌을 표시 합니다. 다음 메시지가 표시 됩니다: *최근 변경 내용으로 인해 충돌이 대체 레이아웃에서*합니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alt 레이아웃 충돌](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt 레이아웃 충돌")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Alt 레이아웃 충돌](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
-----
 

또한 충돌 상자에는 다음과 같은 메시지가 표시 됩니다.

[![충돌 메시지](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

추가 `CheckBox` 때문에 충돌이 발생는 **큰 토지** 레이아웃 변경 되었습니다는 `LinearLayout` 해당 항목을 포함 합니다. 그러나이 경우 표시에 방금 삽입 된 위젯 충돌 상자는 **기본** 레이아웃 (의 `CheckBox`).

클릭 하면 **무시 충돌**, 디자이너에서 충돌을 해결 위젯의 되는 누락 된 끌어서 레이아웃으로 끌어 충돌 상자에 표시 하는 위젯 수 (이 경우는 **큰 토지**  레이아웃): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![그룹 충돌 해결](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "그룹 충돌 해결")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![그룹 충돌 해결](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
-----
 

이전 예제와 같이 `Button`, `CheckBox` 때문에 빨간색 변경 표식 없는는 `LinearLayout` 에 적용 된 변경에는 **큰 토지** 레이아웃 합니다.



### <a name="conflict-persistence"></a>충돌 지 속성

충돌 다음과 같이 XML 주석으로 레이아웃 파일에 유지 됩니다.

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

따라서 프로젝트를 닫았다가, 하는 경우 모든 충돌은 계속 있을 &ndash; 무시 된 경우라도 합니다.

