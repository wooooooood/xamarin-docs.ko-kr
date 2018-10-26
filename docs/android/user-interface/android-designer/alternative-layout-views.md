---
title: 대체 레이아웃 보기
description: 이 항목에서는 어떻게 레이아웃 버전 관리가 가능 리소스 한정자를 사용 하 여 설명 합니다. 예를 들어 장치가 가로 모드에 있을 때만 사용 되는 레이아웃의 버전 및 세로 모드에 대해서만 된 레이아웃 버전이 있을 수 있습니다.
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 03b80d3fb1ed7c8db108f86b3b3923c20e1d908f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109980"
---
# <a name="alternative-layout-views"></a>대체 레이아웃 보기

_이 항목에서는 리소스 한정자를 사용 하 여 버전 레이아웃 하는 방법을 설명 합니다. 예를 들어 장치가 가로 모드에 있을 때만 사용 되는 레이아웃의 버전 및 세로 모드에 대해서만 되는 레이아웃 버전을 만듭니다._

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>대체 레이아웃 만들기

클릭할 때 합니다 **대체 레이아웃 보기** 아이콘 (왼쪽의 **장치**), 프로젝트에서 사용할 수 있는 대체 레이아웃을 나열 하는 미리 보기 창이 열립니다. 대체 레이아웃 없는 경우는 **기본** 뷰가 표시 됩니다. 

[![대체 레이아웃 보기 창](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "대체 레이아웃 보기 창")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

옆에 녹색 더하기 기호를 클릭 하면 **새 버전**서 **레이아웃 변형 만들기** 이 레이아웃 변형에 대 한 리소스 한정자를 선택할 수 있도록 대화 상자가 열립니다. 

[![레이아웃 변형 만들기](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "레이아웃 변형 만들기")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

다음 예에서 대 한 리소스 한정자 **화면 방향** 로 설정 된 **가로**, 및 **화면 크기** 으로 변경 됩니다 **큰**. 라는 새 레이아웃 버전이 이렇게 **land 큰**:

[![큰 land variation](alternative-layout-views-images/vs/03-large-land-sml.png "큰 land 변형")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

왼쪽의 미리 보기 창 표시 리소스 한정자 선택 항목의 효과 참고 합니다. 클릭 **추가** 대체 레이아웃을 만들고 디자이너 레이아웃으로 전환 합니다. 합니다 **대체 레이아웃 보기** 미리 보기 창 다음 스크린샷에 표시 된 대로 각 레이아웃 작은 오른쪽 포인터를 통해 디자이너 로드를 나타냅니다. 

[![로드 된 레이아웃 표시기](alternative-layout-views-images/vs/04-new-layout-sml.png "로드 레이아웃 표시기")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)


## <a name="editing-alternative-layouts"></a>대체 레이아웃 편집

대체 레이아웃을 만들 때 레이아웃의 모든 분기 버전에 적용 되는 단일 변경 하는 것이 좋습니다. 예를 들어, 다음 단추 텍스트를 일부 레이아웃에는 노란색으로 변경 하는 것이 좋습니다. 레이아웃의 많은 수 있고 모든 버전에 단일 변경 내용을 전파 하는 데 필요한 경우 번거롭고 오류가 발생 하기 쉽고 유지 관리 신속 하 게 될 수 있습니다.

디자이너에서는 여러 레이아웃 버전의 유지 관리를 간소화 하는 **다중 편집** 하나 이상의 레이아웃 변경 내용을 전파 하는 모드입니다. 둘 이상의 레이아웃을 사용할 수 있는 경우는 **다중 편집** 아이콘이 표시 됩니다. 

[![다중 편집 아이콘](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "다중 편집 아이콘")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

클릭할 때 합니다 **다중 편집** 아이콘 (아래와 같이) 레이아웃은 연결 되어 있음을 나타내는 선이; 즉, 하나의 레이아웃을 변경한 경우 해당 변경 사항이 전파 됩니다 모든 연결 된 레이아웃에 있습니다. 다음 스크린샷에 표시 된 원형된 아이콘을 클릭 하 여 모든 레이아웃 연결을 끊을 수 있습니다. 

[![모든 레이아웃 연결 해제](alternative-layout-views-images/vs/06-multi-linked-sml.png "모든 레이아웃 연결 해제")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

두 개 이상의 레이아웃에 있는 경우에 레이아웃은 연결을 확인 하려면 각 레이아웃 미리 보기의 왼쪽에 편집 단추를 선택적으로 전환할 수 있습니다. 예를 들어, 전파 하는 단일 변경 하려는 경우 첫 번째 및 세 가지 레이아웃 중 마지막 작업으로 먼저 연결을 해제 하면 중간 레이아웃 여기 표시 된 대로: 

[![중간 레이아웃 연결 해제](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "중간 레이아웃 연결 해제")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

이 예제에서는 변경 중 하나로 합니다 **기본** 또는 **긴** 레이아웃 되지 않습니다 하지만 다른 레이아웃으로 전파 됩니다는 **land 큰** 레이아웃.

### <a name="multi-edit-example"></a>다중 편집 예제 

일반적으로 하나의 레이아웃을 변경한 경우 해당 동일한 변경 내용은 다른 모든 연결 된 레이아웃에 전파 됩니다. 예를 들어, 새를 추가 `TextView` 위젯을 합니다 **기본** 레이아웃 및 변경 된 텍스트 문자열을 `Portrait` 모든 연결 된 레이아웃에 대 한 동일한 변경 사항이 발생 합니다. 여기에서 어떻게 보이는지은 **기본** 레이아웃: 

[![추가 TextView](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView를 추가 합니다.")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
`TextView` 에 추가 됩니다 합니다 **큰 land** 레이아웃 보기에 연결 되어 있으므로 합니다 **기본** 레이아웃: 

[![TextView 가로](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView 가로")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
하지만 하나의 레이아웃에 로컬인 변경 하려는 경우 (즉, 원하지는 다른 레이아웃에 전파 될 변경)? 이렇게 하려면 레이아웃을 수정 하기 전에 다음에 설명 된 대로 변경할에 연결 해제 해야 합니다. 

### <a name="making-local-changes"></a>로컬 변경 

추가 하도록 두 레이아웃 모두 알아보겠습니다 `TextView`에서 텍스트 문자열을 변경 하고자 하지만 합니다 **큰 land** 레이아웃을 `Landscape` 대신 `Portrait`합니다. 이 변경 하는 경우 **큰 land** 변경에 전파 되는 두 레이아웃 모두에 연결 하는 동안 합니다 **기본** 레이아웃 합니다. 따라서 해야 먼저 연결을 해제 두 레이아웃을 변경 하기 전의 합니다. 텍스트를 수정 하는 것 **큰 land** 에 `Landscape`, 디자이너 표시 변경에 로컬 인지 나타내기 위해 빨간색 프레임을 사용 하 여이 변경 합니다 **land 큰** 레이아웃 이며 *하지* 로 다시 전파 합니다 **기본** 레이아웃: 

[![로컬 변경이](alternative-layout-views-images/vs/10-local-change-sml.png "로컬 변경")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
클릭할 때 합니다 **기본** 레이아웃을 볼 합니다 `TextView` 텍스트 문자열은로 설정 되어 `Portrait`합니다. 

## <a name="handling-conflicts"></a>충돌 처리 

에 있는 텍스트의 색을 변경 하려는 경우는 **기본** 녹색으로 레이아웃, 연결 된 레이아웃에 표시 되는 경고 아이콘이 표시 됩니다. 해당 레이아웃을 클릭 하면 충돌을 표시 하기 위해 레이아웃을 열립니다. 충돌을 일으킨 위젯 빨간색 프레임을 사용 하 여 강조 표시 되 고 다음 메시지가 표시 됩니다. *최근 내용으로 인해 충돌이 대체 레이아웃에서*합니다. 

[![충돌 하는 변경](alternative-layout-views-images/vs/11-conflicting-change-sml.png "충돌 하는 변경")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
A *충돌 상자* 충돌을 설명 하기 위해 위젯의 오른쪽에 표시 됩니다. 

[![충돌 경고](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

충돌 상자에서 변경 된 속성의 목록을 표시 하 고 해당 값을 나열 합니다. 클릭 **충돌 무시** 이 위젯을에 속성 변경 내용을 적용 합니다. 클릭 **Apply** 이 위젯을 연결 된 해당 사용자 위젯 속성 변경 내용이 적용 됩니다 **기본** 레이아웃 합니다. 모든 속성 변경이 적용 되는 경우 충돌을 자동으로 삭제 됩니다. 

### <a name="view-group-conflicts"></a>그룹 충돌 보기 

속성 변경만 원인을 충돌 하지 않습니다. 삽입 되거나 위젯 제거 충돌을 검색할 수 있습니다. 예를 들어 경우는 **큰 land** 레이아웃에서 연결이 해제 됩니다는 **기본** 레이아웃 및 `TextView` 에 **큰 land** 레이아웃을 끌어서 위에 놓은 `Button`, 디자이너 표시 충돌을 나타내기 위해 이동된 위젯:

[![그룹 충돌을 보려면](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "그룹 충돌 보기")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
그러나 표식이 없습니다에 있는지를 `Button`입니다. 하지만의 위치를 `Button` 변경 되었습니다를 `Button` 관련이 있는 없습니다 적용 된 변경 내용을 보여 줍니다는 **land 큰** 구성 합니다. 

경우는 `CheckBox` 에 추가 됩니다 합니다 **기본** 레이아웃을 다른 충돌 생성 되 고 경고 아이콘이 표시 되는 **land 큰** 레이아웃: 

[![확인란 충돌](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "확인란 충돌")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
클릭 하는 **land 큰** 레이아웃 충돌을 표시 합니다. 다음과 같은 메시지가 표시 됩니다. *최근 내용으로 인해 충돌이 대체 레이아웃에서*: 

[![대체 레이아웃 충돌](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt 레이아웃 충돌")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

또한 충돌 상자에는 다음과 같은 메시지가 표시 됩니다.

[![충돌 메시지](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

추가 `CheckBox` 때문에 충돌이 발생 합니다 **큰 land** 레이아웃 변경 되었습니다는 `LinearLayout` 포함 된 합니다. 그러나이 경우 표시에 방금 삽입 된 위젯 충돌 상자는 **기본** 레이아웃 (의 `CheckBox`).

클릭 하면 **충돌 무시**, 디자이너에서 충돌을 해결 위젯이 없는을 레이아웃에 끌어서 놓을 수 충돌 상자에 표시 된 위젯 허용 (이 경우에 **큰 land**  레이아웃): 

[![그룹 충돌을 해결](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "그룹 충돌 해결")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

이전 예제에서 볼 수 있듯이 `Button`, `CheckBox` 때문에 빨간색 변경 표식이 없습니다만 합니다 `LinearLayout` 에 적용 된 변경에는 **큰 land** 레이아웃.



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>대체 레이아웃 만들기

클릭할 때 합니다 **대체 레이아웃 보기** 아이콘 (왼쪽의 **장치**), 프로젝트에서 사용할 수 있는 대체 레이아웃을 나열 하는 미리 보기 창이 열립니다. 대체 레이아웃 없는 경우는 **기본** 뷰가 표시 됩니다. 

[![대체 레이아웃 보기 창](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

옆에 녹색 더하기 기호를 클릭 하면 **새 버전**서 **레이아웃 변형 만들기** 이 레이아웃 변형에 대 한 리소스 한정자를 선택할 수 있도록 대화 상자가 열립니다. 

[![레이아웃 변형 만들기](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

다음 예에서 대 한 리소스 한정자 **화면 방향** 로 설정 된 **가로**, 및 **화면 크기** 으로 변경 됩니다 **큰**. 라는 새 레이아웃 버전이 이렇게 **land 큰**:

[![큰 land 변형](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

왼쪽의 미리 보기 창 표시 리소스 한정자 선택 항목의 효과 참고 합니다. 클릭 **추가** 대체 레이아웃을 만들고 디자이너 레이아웃으로 전환 합니다. 합니다 **대체 레이아웃 보기** 미리 보기 창 다음 스크린샷에 표시 된 대로 각 레이아웃 작은 오른쪽 포인터를 통해 디자이너 로드를 나타냅니다. 

[![로드 된 레이아웃 표시기](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>대체 레이아웃 편집

대체 레이아웃을 만들 때 레이아웃의 모든 분기 버전에 적용 되는 단일 변경 하는 것이 좋습니다. 예를 들어, 다음 단추 텍스트를 일부 레이아웃에는 노란색으로 변경 하는 것이 좋습니다. 레이아웃의 많은 수 있고 모든 버전에 단일 변경 내용을 전파 하는 데 필요한 경우 번거롭고 오류가 발생 하기 쉽고 유지 관리 신속 하 게 될 수 있습니다.

디자이너에서는 여러 레이아웃 버전의 유지 관리를 간소화 하는 **다중 편집** 하나 이상의 레이아웃 변경 내용을 전파 하는 모드입니다. 둘 이상의 레이아웃을 사용할 수 있는 경우는 **다중 편집** 아이콘이 표시 됩니다. 

[![다중 편집 아이콘](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

클릭할 때 합니다 **다중 편집** 아이콘 (아래와 같이) 레이아웃은 연결 되어 있음을 나타내는 선이; 즉, 하나의 레이아웃을 변경한 경우 해당 변경 사항이 전파 됩니다 모든 연결 된 레이아웃에 있습니다. 다음 스크린샷에 표시 된 원형된 아이콘을 클릭 하 여 모든 레이아웃 연결을 끊을 수 있습니다. 

[![모든 레이아웃 연결 해제](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

두 개 이상의 레이아웃에 있는 경우에 레이아웃은 연결을 확인 하려면 각 레이아웃 미리 보기의 왼쪽에 편집 단추를 선택적으로 전환할 수 있습니다. 예를 들어, 전파 하는 단일 변경 하려는 경우 첫 번째 및 세 가지 레이아웃 중 마지막 작업으로 먼저 연결을 해제 하면 중간 레이아웃 여기 표시 된 대로: 

[![중간 레이아웃 연결 해제](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

이 예제에서는 변경 중 하나로 합니다 **기본** 또는 **긴** 레이아웃 되지 않습니다 하지만 다른 레이아웃으로 전파 됩니다는 **land 큰** 레이아웃. 

### <a name="multi-edit-example"></a>다중 편집 예제 

일반적으로 하나의 레이아웃을 변경한 경우 해당 동일한 변경 내용은 다른 모든 연결 된 레이아웃에 전파 됩니다. 예를 들어, 새를 추가 `TextView` 위젯을 합니다 **기본** 레이아웃 및 변경 된 텍스트 문자열을 `Portrait` 모든 연결 된 레이아웃에 대 한 동일한 변경 사항이 발생 합니다. 여기에서 어떻게 보이는지은 **기본** 레이아웃: 

[![TextView를 추가 합니다.](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

`TextView` 에 추가 됩니다 합니다 **큰 land** 레이아웃 보기에 연결 되어 있으므로 합니다 **기본** 레이아웃: 

[![가로 TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
하지만 하나의 레이아웃에 로컬인 변경 하려는 경우 (즉, 원하지는 다른 레이아웃에 전파 될 변경)? 이렇게 하려면 레이아웃을 수정 하기 전에 다음에 설명 된 대로 변경할에 연결 해제 해야 합니다. 

### <a name="making-local-changes"></a>로컬 변경 

추가 하도록 두 레이아웃 모두 알아보겠습니다 `TextView`에서 텍스트 문자열을 변경 하고자 하지만 합니다 **큰 land** 레이아웃을 `Landscape` 대신 `Portrait`합니다. 이 변경 하는 경우 **큰 land** 변경에 전파 되는 두 레이아웃 모두에 연결 하는 동안 합니다 **기본** 레이아웃 합니다. 따라서 해야 먼저 연결을 해제 두 레이아웃을 변경 하기 전의 합니다. 텍스트를 수정 하는 것 **큰 land** 에 `Landscape`, 디자이너 표시 변경에 로컬 인지 나타내기 위해 빨간색 프레임을 사용 하 여이 변경 합니다 **land 큰** 레이아웃 이며 *하지* 로 다시 전파 합니다 **기본** 레이아웃: 

[![로컬 변경](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

클릭할 때 합니다 **기본** 레이아웃을 볼 합니다 `TextView` 텍스트 문자열은로 설정 되어 `Portrait`합니다. 

## <a name="handling-conflicts"></a>충돌 처리 

에 있는 텍스트의 색을 변경 하려는 경우는 **기본** 녹색으로 레이아웃, 연결 된 레이아웃에 표시 되는 경고 아이콘이 표시 됩니다. 해당 레이아웃을 클릭 하면 충돌을 표시 하기 위해 레이아웃을 열립니다. 충돌을 일으킨 위젯 빨간색 프레임을 사용 하 여 강조 표시 되 고 다음 메시지가 표시 됩니다. *최근 내용으로 인해 충돌이 대체 레이아웃에서*합니다. 

[![변경 내용이 충돌](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

A *충돌 상자* 충돌을 설명 하기 위해 위젯의 오른쪽에 표시 됩니다. 

[![충돌 경고](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

충돌 상자에서 변경 된 속성의 목록을 표시 하 고 해당 값을 나열 합니다. 클릭 **충돌 무시** 이 위젯을에 속성 변경 내용을 적용 합니다. 클릭 **Apply** 이 위젯을 연결 된 해당 사용자 위젯 속성 변경 내용이 적용 됩니다 **기본** 레이아웃 합니다. 모든 속성 변경이 적용 되는 경우 충돌을 자동으로 삭제 됩니다. 

### <a name="view-group-conflicts"></a>그룹 충돌 보기 

속성 변경만 원인을 충돌 하지 않습니다. 삽입 되거나 위젯 제거 충돌을 검색할 수 있습니다. 예를 들어 경우는 **큰 land** 레이아웃에서 연결이 해제 됩니다는 **기본** 레이아웃 및 `TextView` 에 **큰 land** 레이아웃을 끌어서 위에 놓은 `Button`, 디자이너 표시 충돌을 나타내기 위해 이동된 위젯:

[![보기 그룹 충돌](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
그러나 표식이 없습니다에 있는지를 `Button`입니다. 하지만의 위치를 `Button` 변경 되었습니다를 `Button` 관련이 있는 없습니다 적용 된 변경 내용을 보여 줍니다는 **land 큰** 구성 합니다. 

경우는 `CheckBox` 에 추가 됩니다 합니다 **기본** 레이아웃을 다른 충돌 생성 되 고 경고 아이콘이 표시 되는 **land 큰** 레이아웃: 

[![확인란 충돌](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
클릭 하는 **land 큰** 레이아웃 충돌을 표시 합니다. 다음과 같은 메시지가 표시 됩니다. *최근 내용으로 인해 충돌이 대체 레이아웃에서*합니다. 

[![대체 레이아웃 충돌](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
또한 충돌 상자에는 다음과 같은 메시지가 표시 됩니다.

[![충돌 메시지](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

추가 `CheckBox` 때문에 충돌이 발생 합니다 **큰 land** 레이아웃 변경 되었습니다는 `LinearLayout` 포함 된 합니다. 그러나이 경우 표시에 방금 삽입 된 위젯 충돌 상자는 **기본** 레이아웃 (의 `CheckBox`).

클릭 하면 **충돌 무시**, 디자이너에서 충돌을 해결 위젯이 없는을 레이아웃에 끌어서 놓을 수 충돌 상자에 표시 된 위젯 허용 (이 경우에 **큰 land**  레이아웃): 

[![그룹 충돌 해결](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
이전 예제에서 볼 수 있듯이 `Button`, `CheckBox` 때문에 빨간색 변경 표식이 없습니다만 합니다 `LinearLayout` 에 적용 된 변경에는 **큰 land** 레이아웃.


-----


### <a name="conflict-persistence"></a>충돌 지 속성

충돌 다음과 같이 레이아웃 파일에서 XML 주석으로 유지 됩니다.

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

따라서 프로젝트를 닫은 후에 하는 경우 모든 충돌은 계속 있을 &ndash; 무시 된 것도 합니다.



