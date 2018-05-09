---
title: Android 디자이너를 사용 하 여
description: 이 항목은 Xamarin.Android 디자이너의 연습 과정입니다. 소형 컬러 브라우저 응용 프로그램에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스 디자이너에서 전적으로 만들어집니다.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/10/2018
ms.openlocfilehash: 8d1dc410d5336d9c2505a18720cc7f734e838c39
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="using-the-android-designer"></a>Android 디자이너를 사용 하 여

_이 항목은 Xamarin.Android 디자이너의 연습 과정입니다. 소형 컬러 브라우저 응용 프로그램에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스 디자이너에서 전적으로 만들어집니다._


## <a name="overview"></a>개요

Android 사용자 인터페이스 XML 파일을 사용 하 여 또는 프로그래밍 방식으로 코드를 작성 하 여 선언적으로 만들 수 있습니다. Xamarin.Android 개발자가을 만들고 XML 파일을 직접 편집 건을 다루는 데 필요 없이 선언적 레이아웃을 시각적으로 수정할 수 있습니다. 디자이너는 또한 개발자는 장치 또는 에뮬레이터에 응용 프로그램을 다시 배포할 필요 없이 UI 변경 내용을 평가할 수 있는 실시간 피드백을 제공 합니다. 이 속도를 높일 수 Android UI 개발 매우 합니다. 이 문서에서는 Xamarin.Android 디자이너 사용자 인터페이스를 시각적으로 만들를 사용 하는 방법을 보여 주는 연습을 소개 합니다.


## <a name="walkthrough"></a>연습

이 연습에서는 Android 디자이너를 사용 하 여 색, 이름 및 해당 RGB 값의 목록을 제공 하는 예제 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 것입니다. 디자인 화면에 위젯을 추가 하는 방법 뿐 아니라 이러한 위젯 시각적으로 배치 하는 방법을 살펴보겠습니다. 그 후 디자이너를 사용 하 여 또는 디자인 화면에서 대화형으로 위젯 수정 하는 방법을 설명 합니다 **속성 패드**합니다. 마지막으로 응용 프로그램을 실행할 때 디자인 표시 되는 모양을 표시 됩니다.

이제 시작 하겠습니다.


### <a name="creating-a-new-project"></a>새 프로젝트 만들기

첫 번째 단계는 새 Xamarin.Android 프로젝트를 만드는 것입니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio를 시작 하 고 클릭 **새 프로젝트...**  선택 합니다는 **Visual C\# > Android > Android 앱 (Xamarin)** 템플릿:

[![새 android 응용 프로그램](designer-walkthrough-images/vs/01-android-app-sml.w157.png)](designer-walkthrough-images/vs/01-android-app.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 및 클릭에 대 한 Visual Studio를 시작 **새 솔루션 중...** . 선택 된 **Android 앱** 템플릿을 **다음**:

[![새 android 응용 프로그램](designer-walkthrough-images/xs/01-android-app-sml.png)](designer-walkthrough-images/xs/01-android-app.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

새 앱의 이름을 **DesignerWalkthrough** 클릭 **확인**합니다.

[![응용 프로그램 이름](designer-walkthrough-images/vs/02-name-app-sml.png)](designer-walkthrough-images/vs/02-name-app.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

새 앱의 이름을 **DesignerWalkthrough**합니다. 아래 **대상 플랫폼**선택, **최신 정보 및 가장** 클릭 **다음**:

[![응용 프로그램 이름](designer-walkthrough-images/xs/02-designer-walkthrough-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough.png#lightbox)

다음 대화 상자 화면에서 클릭 **만들기**합니다.

-----



### <a name="adding-a-layout"></a>레이아웃을 추가합니다.

만들기는 **LinearLayout** 는 사용 하 여 사용자 인터페이스 요소를 보유할 수 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio에서 마우스 오른쪽 단추로 클릭 **리소스/레이아웃** 에 **솔루션 탐색기** 선택 **추가 > 새 항목...** . 에 **새 항목 추가** 대화 상자에서 **Android 레이아웃**합니다. 파일 이름을 **ListItem.axml** 클릭 **추가**:

[![새 레이아웃](designer-walkthrough-images/vs/03-new-layout-sml.w157.png)](designer-walkthrough-images/vs/03-new-layout.w157.png#lightbox)

새 **ListItem** 레이아웃이 디자이너에 표시 됩니다.

[![디자이너 뷰](designer-walkthrough-images/vs/04-designer-view-sml.png)](designer-walkthrough-images/vs/04-designer-view.png#lightbox)

클릭는 **소스** 이 레이아웃에 대 한 XML 원본을 보려면 디자이너의 아래쪽에 탭:

[![XML 디자이너](designer-walkthrough-images/vs/05-designer-xml-sml.png)](designer-walkthrough-images/vs/05-designer-xml.png#lightbox)

**보기** 메뉴를 클릭 하 여 **다른 창 > 문서 개요** 열려는 **문서 개요**합니다. **문서 개요** 레이아웃 단일 현재 포함 되어 있음을 보여 줍니다. **LinearLayout** 위젯:

[![문서 개요](designer-walkthrough-images/vs/06-document-outline-sml.png)](designer-walkthrough-images/vs/06-document-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **리소스/레이아웃** 에 **솔루션** 패딩 하 고 선택 **추가 > 새 파일...** . 에 **새 파일** 대화 상자에서 **Android > 레이아웃**합니다. 파일 이름을 **ListItem** 클릭 **새로**:

[![새 레이아웃](designer-walkthrough-images/xs/03-new-layout-sml.png)](designer-walkthrough-images/xs/03-new-layout.png#lightbox)

새 **ListItem** 레이아웃이 디자이너에 표시 됩니다.

[![디자이너 뷰](designer-walkthrough-images/xs/04-designer-view-sml.png)](designer-walkthrough-images/xs/04-designer-view.png#lightbox)

클릭는 **소스** 이 레이아웃에 대 한 XML 원본을 보려면 디자이너의 아래쪽에 탭 합니다. 클릭는 **문서 개요** 탭 오른쪽에 레이아웃 단일을 현재 포함 된 표시 **LinearLayout** 위젯:

[![XML 디자이너](designer-walkthrough-images/xs/05-designer-xml-sml.png)](designer-walkthrough-images/xs/05-designer-xml.png#lightbox)

-----



### <a name="creating-the-list-item-user-interface"></a>목록 항목 사용자 인터페이스 만들기

다음으로, 하겠습니다 색 브라우저 앱에 대 한 사용자 인터페이스를 만들 수 있습니다.
클릭는 **디자이너** 탭에서 디자인 화면으로 돌아갑니다.
에 **도구 상자**,으로 아래로 스크롤하여는 **이미지 및 미디어** 섹션 및 찾을 때까지 각 항목을 살펴보고는 *ImageView*:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView 찾기](designer-walkthrough-images/vs/07-locate-imageview-sml.png)](designer-walkthrough-images/vs/07-locate-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView 찾기](designer-walkthrough-images/xs/06-locate-imageview-sml.png)](designer-walkthrough-images/xs/06-locate-imageview.png#lightbox)

-----

입력할 수 또는 *ImageView* 찾으려고 검색 표시줄에는 `ImageView` 위젯:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView 검색](designer-walkthrough-images/vs/08-imageview-search-sml.png)](designer-walkthrough-images/vs/08-imageview-search.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView 검색](designer-walkthrough-images/xs/07-imageview-search-sml.png)](designer-walkthrough-images/xs/07-imageview-search.png#lightbox)

-----

이 드래그 `ImageView` 디자인 화면으로:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![캔버스에서 ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![캔버스에서 ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas.png#lightbox)

-----

이 `ImageView` 색 견본 색 브라우저 앱에 표시 하는 데 사용 됩니다.

를 끌어 한 `LinearLayout (Vertical)` 에서 위젯은 **도구 상자** 디자이너에 있습니다. 파란색 개요 있음을 나타내도록 경계는 추가 참고 `LinearLayout`, 및 **문서 개요** 표시 바로 아래 있는 `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![파란색 개요](designer-walkthrough-images/vs/10-blue-outline-sml.png)](designer-walkthrough-images/vs/10-blue-outline.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![파란색 개요](designer-walkthrough-images/xs/10-blue-outline-sml.png)](designer-walkthrough-images/xs/10-blue-outline.png#lightbox)

-----

선택 하는 경우는 `ImageView` 디자이너에서 파란색 개요를 묶는 이동는 `ImageView`;에 **문서 개요**, 선택 이동 `imageView1 (ImageView)`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ImageView 선택](designer-walkthrough-images/vs/11-select-imageview-sml.png)](designer-walkthrough-images/vs/11-select-imageview.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ImageView 선택](designer-walkthrough-images/xs/11-select-imageview-sml.png)](designer-walkthrough-images/xs/11-select-imageview.png#lightbox)

-----

를 끌어 한 `Text (Large)` 에서 위젯은 **도구 상자** 새로 추가한에 `LinearLayout`합니다. 디자이너 녹색을 사용 하는 새 위젯에 삽입 될 위치를 강조 표시 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![녹색 강조 표시](designer-walkthrough-images/vs/12-green-highlight-sml.png)](designer-walkthrough-images/vs/12-green-highlight.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![녹색 강조 표시](designer-walkthrough-images/xs/12-green-highlight-sml.png)](designer-walkthrough-images/xs/12-green-highlight.png#lightbox)

-----

다음으로 추가 된 `Text (Small)` 위젯 아래는 `Text (Large)` 위젯:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![작은 텍스트 위젯 추가](designer-walkthrough-images/vs/13-add-small-text-sml.png)](designer-walkthrough-images/vs/13-add-small-text.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![작은 텍스트 위젯 추가](designer-walkthrough-images/xs/13-add-small-text-sml.png)](designer-walkthrough-images/xs/13-add-small-text.png#lightbox)

-----

이 시점에서 디자이너에는 다음 스크린 샷에서 유사 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자이너 레이아웃](designer-walkthrough-images/vs/14-raw-layout-sml.png)](designer-walkthrough-images/vs/14-raw-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자이너 레이아웃](designer-walkthrough-images/xs/14-raw-layout-sml.png)](designer-walkthrough-images/xs/14-raw-layout.png#lightbox)

-----

경우 두 `textView` widget를 안쪽 없는 `linearLayout1`에으로 끌 수 있습니다 `linearLayout1` 에 **문서 개요** 하 고 이전 스크린 샷에 표시 된 것 처럼 표시 위치 (아래 들여쓰기 되어 `linearLayout1`).



### <a name="arranging-the-user-interface"></a>사용자 인터페이스를 정렬합니다.

표시 하는 UI를 수정 해 보겠습니다는 `ImageView` 왼쪽에서 두 개의 `TextView` 위젯 누적의 오른쪽에는 `ImageView`합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  `ImageView`를 선택합니다.

2.  에 **속성 창**, 클릭는 **항목별** 정렬 아이콘 및로 스크롤하여는 **레이아웃-보기 그룹** 섹션.

3.  변경 된 `layout_width` 설정을 `wrap_content`:

![래핑 콘텐츠 설정](designer-walkthrough-images/vs/15-wrap-content.png "집합 래핑 콘텐츠")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  와 `ImageView` 선택한 클릭는 **속성** 탭 합니다.

2.  바로 아래에서 **속성** 탭을 클릭 **레이아웃**합니다.

3.  으로 아래로 스크롤하여 **보기 그룹** 변경 하 고는 `Width` 설정을 `wrap_content`:

[![집합 래핑 콘텐츠](designer-walkthrough-images/xs/15-wrap-content-sml.png)](designer-walkthrough-images/xs/15-wrap-content.png#lightbox)

-----

변경할 수는 `Width` 설정은 해당 너비 설정을를 전환 하려면 위젯의 오른쪽에 있는 삼각형을 클릭 하는 `wrap_content`:

[![끌어서 너비를 설정 하려면](designer-walkthrough-images/xs/16-width-arrow-sml.png)](designer-walkthrough-images/xs/16-width-arrow.png#lightbox)

반환 삼각형을 다시 클릭 하 고 `Width` 설정을 `match_parent`합니다.

다음으로 전환 하는 **문서 개요** 루트를 선택 하 고 `LinearLayout`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![루트 LinearLayout 선택](designer-walkthrough-images/vs/16-root-linearlayout-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![루트 LinearLayout 선택](designer-walkthrough-images/xs/17-root-linearlayout-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout.png#lightbox)

-----
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

루트와 `LinearLayout` 돌아가려면 선택 된 **속성** 창 클릭는 **사전순** 정렬 아이콘 및 스크롤을 찾을 때까지 `orientation`합니다. 변경 된 `orientation` 설정을 `horizontal`:

![가로 방향 선택](designer-walkthrough-images/vs/17-horizontal-orientation.png "가로 방향을 선택 합니다.")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

루트와 `LinearLayout` 돌아가려면 선택 된 **속성** 탭을 클릭 **위젯**합니다. 변경 된 `Orientation` 설정을 `horizontal`:

[![가로 방향을 선택 합니다.](designer-walkthrough-images/xs/18-horizontal-orientation-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation.png#lightbox)

-----

이 시점에서 디자이너에는 다음 스크린 샷에서 유사 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![디자이너 레이아웃](designer-walkthrough-images/vs/18-designer-layout-sml.png)](designer-walkthrough-images/vs/18-designer-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![디자이너 레이아웃](designer-walkthrough-images/xs/19-designer-layout-sml.png)](designer-walkthrough-images/xs/19-designer-layout.png#lightbox)

-----


### <a name="modifying-the-spacing"></a>간격을 수정합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

다음으로 위젯 사이 공백을 더 제공 하기 위해 UI에 안쪽 여백 및 여백 설정을 수정 합니다. 선택의 `ImageView`, 클릭는 **항목별** 검색 아이콘에는 **속성** 창과로 스크롤하여는 **레이아웃** 섹션. 변경의 `Min Height` 를 `70dp`, `Min Width` 를 `50dp`, 및 `padding` 를 `10dp`합니다. 모든 면 주위의 여백이 적용 됩니다는 `ImageView` 세로로 elongates 및:

[![안쪽 여백 설정](designer-walkthrough-images/vs/19-padding-widths-sml.png)](designer-walkthrough-images/vs/19-padding-widths.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

다음으로 위젯 사이 공백을 더 제공 하기 위해 UI에 안쪽 여백 및 여백 설정을 수정 합니다. 선택 된 `ImageView` 클릭는 **레이아웃** 탭의 **속성**합니다. 변경의 `Padding` 를 `10dp`, `Min Width` 를 `50dp`, 및 `Min Height` 를 `70dp`합니다. 모든 면 주위의 여백이 적용 됩니다는 `ImageView` 세로로 elongates 및:

[![안쪽 여백 설정](designer-walkthrough-images/xs/20-padding-widths-sml.png)](designer-walkthrough-images/xs/20-padding-widths.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

아래, 왼쪽, 오른쪽 안쪽 여백 설정이 위쪽에 값을 입력 하 여 독립적으로 설정할 수 있습니다는 `paddingBottom`, `paddingLeft`, `paddingRight`, 및 `paddingTop` 필드를 각각.
예를 들어 설정는 `paddingLeft` 필드를 `5dp` 및 `paddingBottom`, `paddingRight`, 및 `paddingTop` 필드를 `10dp`:

[![사용자 지정 안쪽 여백 설정](designer-walkthrough-images/vs/20-custom-padding-sml.png)](designer-walkthrough-images/vs/20-custom-padding.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

위쪽, 오른쪽, 아래쪽 및 왼쪽 설정을 안쪽 여백을 설정할 수 있습니다 독립적으로 값을 입력 하 여는 `Top`, `Right`, `Bottom`, 및 `Left` 필드를 각각 패딩 합니다. 예를 들어 설정는 `Left` 안쪽 여백 값을 `5dp` 및 `Top`, `Right`, 및 `Bottom` 안쪽 여백 값을 `10dp`합니다. `Padding` 설정이 이러한 값의 쉼표로 구분 된 목록으로 변경 합니다.

[![사용자 지정 안쪽 여백 설정](designer-walkthrough-images/xs/21-custom-padding-sml.png)](designer-walkthrough-images/xs/21-custom-padding.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

위치를 수정 하는 다음으로 `LinearLayout` 두 포함 된 위젯을 `TextView` 위젯입니다. 에 **문서 개요**선택, `linearLayout1`합니다. 에 **속성** 창에서는 **레이아웃-보기 그룹** 섹션. 설정 `layout_marginBottom`, `layout_marginLeft`, `layout_marginRight`, 및 `layout_marginTop` 를 `5dp`, `5dp`, `0dp`, 및 `5dp` 각각:

[![여백 설정](designer-walkthrough-images/vs/21-margins-sml.png)](designer-walkthrough-images/vs/21-margins.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

위치를 수정 하는 다음으로 `LinearLayout` 두 포함 된 위젯을 `TextView` 위젯입니다. 에 **문서 개요**선택, `linearLayout1`합니다. 에 **속성** 창, 선택는 **레이아웃** 탭 합니다. 으로 아래로 스크롤하여는 **보기 그룹** 섹션 및 설정의 `Left`, `Top`, `Right`, 및 `Bottom` 를 여백 `5dp`, `5dp`, `0dp`, 및 `5dp` 각각:

[![여백 설정](designer-walkthrough-images/xs/22-margins-sml.png)](designer-walkthrough-images/xs/22-margins.png#lightbox)

-----



### <a name="removing-the-default-image"></a>기본 이미지를 제거합니다.

사용 하기 때문에 `ImageView` 색 (이미지 아님)를 표시 하려면 템플릿에 의해 추가 되는 기본 이미지 원본 제거 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  `ImageView`를 선택합니다.

2.  **속성**, 찾을 `src` 필드입니다.

3.  선택을 취소는 `src` 목록이 비어 있도록 설정 합니다.

![ImageView src 설정의 선택을 취소](designer-walkthrough-images/vs/22-clear-img-src.png "ImageView src 설정의 선택을 취소")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  `ImageView`를 선택합니다.

2.  클릭는 **위젯** 탭의 **속성**합니다.

3.  선택을 취소는 `Src` 목록이 비어 있도록 설정 합니다.

[![ImageView src 설정의 선택을 취소합니다](designer-walkthrough-images/xs/23-clear-src-sml.png)](designer-walkthrough-images/xs/23-clear-src.png#lightbox)

-----


### <a name="adding-a-listview-container"></a>ListView 컨테이너 추가 중

이제는 **ListItem** 추가 하겠습니다 레이아웃이 정의 되는 `ListView` Main 레이아웃으로 합니다. 이 `ListView` 의 목록이 포함 됩니다 **ListItems**합니다.
에 **도구 상자**를 찾습니다는 `ListView` 위젯 디자인 화면으로 끌어 옵니다. `ListView` 디자이너에서 비게 됩니다 선택 된 경우의 테두리에 간략하게 설명 하는 파란색 선 제외 하 고 있습니다. 볼 수는 **문서 개요** 되었는지 확인 하는 **ListView** 이 제대로 추가:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![새 ListView](designer-walkthrough-images/vs/23-new-listview.png "새 ListView")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![새 ListView](designer-walkthrough-images/xs/24-new-listview-sml.png)](designer-walkthrough-images/xs/24-new-listview.png#lightbox)

-----

기본적으로는 `ListView` 지정는 `Id` 값 `@+id/listView1`합니다.
열기는 **위젯** 탭의 **속성** 변경 하 고는 `Id` 를 `@+id/myListView`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![MyListView id의 이름을](designer-walkthrough-images/vs/24-change-id.png "myListView id 이름 바꾸기")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyListView id의 이름을](designer-walkthrough-images/xs/25-change-id-sml.png)](designer-walkthrough-images/xs/25-change-id.png#lightbox)

-----

이 시점에서 사용자 인터페이스를 사용할 수는 있습니다.



### <a name="running-the-application"></a>응용 프로그램 실행


열기 **MainActivity.cs** 해당 코드는 다음과 같이 바꿉니다.

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Runtime;
using Android.Views;
using Android.Widget;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity (Label = "DesignerWalkthrough", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem> ();
        ListView listView;

        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);

            SetContentView (Resource.Layout.Main);
            listView = FindViewById<ListView> (Resource.Id.myListView);

            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.DarkRed,
                                               ColorName = "Dark Red", Code = "8B0000" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.SlateBlue,
                                               ColorName = "Slate Blue", Code = "6A5ACD" });
            colorItems.Add (new ColorItem () { Color = Android.Graphics.Color.ForestGreen,
                                               ColorName = "Forest Green", Code = "228B22" });

            listView.Adapter = new ColorAdapter (this, colorItems);
        }
    }
    public class ColorAdapter : BaseAdapter<ColorItem>
    {
        List<ColorItem> items;
        Activity context;
        public ColorAdapter(Activity context, List<ColorItem> items)
            : base()
        {
            this.context = context;
            this.items = items;
        }
        public override long GetItemId(int position)
        {
            return position;
        }
        public override ColorItem this[int position]
        {
            get { return items[position]; }
        }
        public override int Count
        {
            get { return items.Count; }
        }
        public override View GetView (int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.ListItem, null);
            view.FindViewById<TextView>(Resource.Id.textView1).Text = item.ColorName;
            view.FindViewById<TextView>(Resource.Id.textView2).Text = item.Code;
            view.FindViewById<ImageView>(Resource.Id.imageView1).SetBackgroundColor(item.Color);

            return view;
        }
    }

    public class ColorItem
    {
        public string ColorName { get; set; }
        public string Code { get; set; }
        public Android.Graphics.Color Color { get; set; }
    }
}

```

이 코드는 사용자 지정을 사용 하 여 `ListView` 어댑터 색 정보를 로드 하 고 우리가 방금 만든 UI에이 데이터를 표시 합니다. 이 예에서는 간단한을 유지 하기 위해 색상 정보 목록으로 하드 코딩 된 이지만를 데이터 원본에서 색 정보를 추출 하거나 즉시 계산 하는 어댑터를 수정할 수 있습니다. 에 대 한 자세한 내용은 `ListView` 어댑터, 참조 [Listview 및 어댑터](~/android/user-interface/layouts/list-view/index.md)합니다.

응용 프로그램을 빌드 및 실행합니다. 다음 스크린 샷에서 장치에서 실행 하는 경우 응용 프로그램이 표시 하는 방법의 예시:

[![최종 스크린 샷](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)



## <a name="summary"></a>요약

이 문서에서는 사용자 인터페이스를 만드는 Mac 용 Visual Studio에서 Xamarin.Android 디자이너를 사용 하는 방법을 단계별로 진행할 합니다. 다음으로, म 목록에서 단일 항목에 대 한 인터페이스를 만드는 방법을 보여 줍니다.
프로세스를 진행 자원을 배정 하 고 해당 위젯을의 다양 한 속성을 설정 하는 방법 뿐만 아니라 위젯을 추가 하 고 배치 하 여 시각적으로 하는 방법을 살펴보았습니다. 결론적으로 디자이너에서 만든 인터페이스는 예제 응용 프로그램에서 실행 하는 방법을 설명 합니다.
