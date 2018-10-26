---
title: Xamarin.Android 디자이너를 사용 하 여
description: 이 문서는 Xamarin.Android 디자이너의 연습입니다. 작은 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스 디자이너에서 완전히 생성 됩니다.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1174fe5cb417d4977fd6519086e6c4942e74c10b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120276"
---
# <a name="using-the-xamarinandroid-designer"></a>Xamarin.Android 디자이너를 사용 하 여

_이 문서는 Xamarin.Android 디자이너의 연습입니다. 작은 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스 디자이너에서 완전히 생성 됩니다._


## <a name="overview"></a>개요

XML 파일을 사용 하 여 또는 프로그래밍 방식으로 코드를 작성 하 여 android 사용자 인터페이스를 선언적으로 만들 수 있습니다. Xamarin.Android 디자이너에는 개발자를를 만들고 XML 파일의 직접 편집 하지 않고도 선언적 레이아웃을 시각적으로 수정할 수 있습니다. 디자이너는 또한 개발자 장치 또는 에뮬레이터에 응용 프로그램을 재배포 하지 않고도 UI 변경 내용을 평가할 수 있는 실시간 피드백을 제공 합니다. 이러한 디자이너 기능 속도 높일 수 Android UI 개발 단시간 합니다.
이 문서에서는 Xamarin.Android 디자이너를 사용 하 여 시각적으로 사용자 인터페이스를 만드는 방법을 보여 줍니다.

## <a name="walkthrough"></a>연습

이 연습에서는 색 브라우저 앱의 예에 대 한 사용자 인터페이스를 만드는 Android Designer를 사용 하는 것입니다. 색 브라우저 앱에는 색, 이름 및 해당 RGB 값의 목록을 표시합니다. widget을 추가 하는 방법을 알아봅니다 합니다 **디자인 화면** 이러한 위젯에 시각적으로 레이아웃 하는 방법 뿐만 아니라 합니다. 그 후에 대화형으로 위젯을 수정 하는 방법을 알아봅니다 합니다 **디자인 화면** 또는 디자이너를 사용 하 여 **속성** 창입니다. 마지막으로, 장치 또는 에뮬레이터에서 앱을 실행할 때 디자인 표시 하는 방법에 대해 알아봅니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

첫 번째 단계는 새 Xamarin.Android 프로젝트를 만드는 것입니다. Visual Studio를 시작, 클릭 **새 프로젝트...** 를 선택 합니다 **Visual C\# > Android > Android 앱 (Xamarin)** 템플릿.
새 앱 이름을 **DesignerWalkthrough** 누릅니다 **확인**합니다.

[![비어 있는 android 앱](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

에 **새 Android 앱** 대화 상자에서 선택 **비어 있는 앱** 누릅니다 **확인**:

[![비어 있는 Android 앱 템플릿 선택](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>레이아웃을 추가합니다.

다음 단계를 만드는 것을 **LinearLayout** 를 보유 하는 사용자 인터페이스 요소입니다. 마우스 오른쪽 단추로 클릭 **리소스/레이아웃** 에 **솔루션 탐색기** 선택한 **추가 > 새 항목...** . 에 **새 항목 추가** 대화 상자에서 **Android 레이아웃**합니다. 파일 이름을 **list_item** 누릅니다 **추가**:

[![새 레이아웃](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

새 **list_item** 레이아웃 디자이너에 표시 됩니다. 두 개의 창이 표시 됩니다 &ndash; 는 *디자인 화면* 에 대 한 합니다 **list_item** 해당 XML 원본은 오른쪽 창에 표시 되는 동안 왼쪽된 된 창에 표시 됩니다. 위치를 바꿀 수 있습니다는 **디자인 화면** 하 고 **원본** 창을 클릭 하 여는 **스왑 창** 아이콘 두 창 사이:

[![디자이너 뷰](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

**보기** 메뉴에서 클릭 **다른 Windows > 문서 개요** 열려는 합니다 **문서 개요**합니다. 합니다 **문서 개요** 레이아웃에 현재 단일 포함 표시 **LinearLayout** 위젯:

[![문서 개요](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

이 색 브라우저 앱에 대 한 사용자 인터페이스를 만들려면 다음 단계는 `LinearLayout`합니다.

### <a name="creating-the-list-item-user-interface"></a>목록 항목 사용자 인터페이스 만들기

경우는 **도구 상자** 창이 보이지 않으면를 클릭 합니다 **도구 상자** 왼쪽 탭 합니다. 에 **도구 상자**, 아래로 스크롤하여는 **이미지 및 미디어** 섹션을 찾을 때까지 더 아래로 스크롤하여는 `ImageView`:

[![ImageView 찾기](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

또는 입력할 수 있습니다 *ImageView* 찾으려고 검색 표시줄에는 `ImageView`:

[![ImageView 검색](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

이 드래그 `ImageView` 디자인 화면 (이 `ImageView` 색 견본 색 브라우저 앱에서 표시 하는 데 사용할):

[![캔버스에서 ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

를 끌어를 `LinearLayout (Vertical)` 에서 위젯 합니다 **도구 상자** 디자이너로 합니다. 파란색 윤곽선 추가한 경계 나타냅니다 `LinearLayout`합니다. 합니다 **문서 개요** 의 자식 임을 `LinearLayout`아래에 있는 `imageView1 (ImageView)`:

[![파란색 윤곽선](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

선택 하는 경우는 `ImageView` 디자이너에서를 이동 하는 파란색 윤곽선을 `ImageView`입니다. 또한, 선택 영역을 이동할 `imageView1 (ImageView)` 에 **문서 개요**:

[![ImageView 선택](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

를 끌어를 `Text (Large)` 에서 위젯 합니다 **도구 상자** 에 새로 추가 된 `LinearLayout`합니다. 새 위젯 삽입 될 위치를 나타내기 위해 알림 디자이너는 녹색 강조 표시 합니다.

[![녹색 강조 표시](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

다음으로, 추가 된 `Text (Small)` 위젯 아래는 `Text (Large)` 위젯:

[![작은 텍스트 위젯 추가](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

이 시점에서 디자이너 화면에 다음 스크린샷과 유사 해야 합니다.

[![디자이너 레이아웃](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

하는 경우 두 `textView` 위젯은 inside `linearLayout1`를 끌어올 수 있습니다 `linearLayout1` 에 **문서 개요** 와 이전 스크린샷에 표시 된 대로 나타나도록 위치를 (보다 한 수준 아래 `linearLayout1`).


### <a name="arranging-the-user-interface"></a>사용자 인터페이스를 정렬합니다.

UI 표시를 수정 하려면 다음 단계는 합니다 `ImageView` 왼쪽에서 두 개의 `TextView` 위젯 오른쪽의 누적는 `ImageView`합니다.

1.  `ImageView`를 선택합니다.

2.  에 **속성 창**를 입력 *너비* 검색에서 상자를 찾습니다 **레이아웃 너비**합니다.

3.  변경 된 **레이아웃 너비** 설정을 `wrap_content`:

![줄 바꿈 콘텐츠 집합](designer-walkthrough-images/vs/15-wrap-content-w158.png)

변경 하는 또 다른 방법은 합니다 `Width` 설정은 해당 너비 설정을 전환 하려면 위젯의 오른쪽에 있는 삼각형을 클릭 하는 것 `wrap_content`:

![끌어서 너비를 설정 하려면](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

반환 삼각형을 다시 클릭 하 여 `Width` 로 설정 `match_parent`합니다. 다음으로 이동 합니다 **문서 개요** 창과 선택 루트 `LinearLayout`:

[![LinearLayout 루트를 선택 합니다.](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

루트를 사용 하 여 `LinearLayout` 돌아가기 옵션을 선택 합니다 **속성** 창 입력 *방향* 검색에 찾아서 상자를 **방향** 설정 합니다. 변경 **방향을** 에 `horizontal`:

![가로 방향을 선택합니다](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

이 시점에서 디자이너 화면에 다음 스크린샷과 유사 합니다.
다음에 유의 합니다 `TextView` 위젯을 오른쪽으로 이동 되었습니다는 `ImageView`:

[![디자이너 레이아웃](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>간격 수정

위젯 간에 더 많은 공간을 제공 하려면 UI의 안쪽 여백 및 여백 설정을 수정 하려면 다음 단계가입니다. 선택 된 `ImageView` 디자인 화면에서 합니다. 에 **속성** 창 입력 `min` 검색 상자에 있습니다. 입력 `70dp` 에 대 한 **최소 높이** 하 고 `50dp` 에 대 한 **최소 너비**:

[![너비 및 높이 설정](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

에 **속성** 창 입력 `padding` 검색에서 상자 하 고 입력 `10dp` 에 대 한 **패딩**합니다. 이러한 `minHeight`, `minWidth` 하 고 `padding` 설정의 모든 면 주위에 안쪽 여백을 추가 `ImageView` 이 세로 방향으로 늘리면 및 합니다. 이러한 값을 입력 하면 XML 레이아웃 변경 되는지 확인 합니다.

[![요소의 안쪽 여백 설정](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

아래쪽, 왼쪽, 오른쪽 안쪽 여백 설정이 위쪽에 값을 입력 하 여 독립적으로 설정할 수 있습니다 합니다 **아래쪽 안쪽 여백**를 **왼쪽 안쪽 여백을**를 **오른쪽 안쪽 여백을**, 및  **위쪽 여백** 필드를 각각.
예를 들어 설정 합니다 **왼쪽 안쪽 여백** 필드를 `5dp` 및 **아래쪽 안쪽 여백**를 **오른쪽 안쪽 여백을**, 및 **위쪽 안쪽 여백** 필드 `10dp`:

[![사용자 지정 패딩 설정](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

그런 다음의 위치를 조정 합니다 `LinearLayout` 두 가지를 포함 하는 위젯 `TextView` 위젯입니다. 에 **문서 개요**, 선택 `linearLayout1`합니다. 에 **속성** 창에서 입력 `margin` 검색 상자에 있습니다. 설정 **레이아웃 여백 아래쪽**를 **레이아웃 여백 왼쪽**, 및 **레이아웃 여백 위쪽** 에 `5dp`입니다. 설정할 **레이아웃 여백 오른쪽** 에 `0dp`:

[![여백 설정](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>기본 이미지를 제거합니다.

때문에 `ImageView` 색을 표시 하는 데 사용 되는 템플릿에 의해 추가 된 기본 이미지 소스를 제거 하려면 다음 단계는 (이미지)를 대신 합니다.

1.  선택 된 `ImageView` 에 **디자이너 화면**합니다.

2.  **속성**를 입력 *src* 검색 상자에 있습니다.

3.  오른쪽의 작은 사각형을 클릭 합니다 **Src** 속성 설정 선택 **재설정**:

[![ImageView src 설정의 선택을 취소합니다](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

이 제거 `android:src="@android:drawable/ic_menu_gallery"` 소스 XML에서에서 해당 `ImageView`합니다.

### <a name="adding-a-listview-container"></a>ListView 컨테이너 추가

이제는 **list_item** 레이아웃 정의 된, 다음 단계를 추가 하는 것을 `ListView` 기본 레이아웃으로 합니다. 이렇게 `ListView` 목록이 포함 됩니다 **list_item**합니다. 

에 **솔루션 탐색기**오픈 **Resources/layout/activity_main.axml**합니다. 에 **도구 상자**를 찾습니다는 `ListView` 위젯 끕니다를 **디자인 화면**합니다. `ListView` 디자이너에서 비어 있게 됩니다 선택 될 때 해당 테두리를 간략하게 설명 하는 파란색 선은 제외 하 고 있습니다. 볼 수 있습니다는 **문서 개요** 되었는지 확인 하는 **ListView** 가 올바르게 추가:

[![새 ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

기본적으로 `ListView` 같습니다는 `Id` 값 `@+id/listView1`합니다.
하는 동안 `listView1` 가 선택 된 상태로 합니다 **문서 개요**엽니다는 **속성** 창 클릭 **정렬 기준**, 선택한 **범주**.
열기 **Main**를 찾습니다는 **Id** 속성 값을 변경 하 고 `@+id/myListView`:

[![MyListView id의 이름을](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

이 시점에서 사용자 인터페이스를 사용할 준비가 되었습니다.

### <a name="running-the-application"></a>응용 프로그램 실행

오픈 **MainActivity.cs** 및 해당 코드를 다음으로 바꿉니다.

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using Android.Support.V7.App;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

이 코드에서는 사용자 지정 `ListView` 어댑터 색 정보를 로드 하 고 방금 만든 UI에이 데이터를 표시 합니다. 을 유지 하기 위해이 예제에서는 짧은 색 정보를 목록에 하드 코딩 되었지만를 데이터 원본에서 색 정보를 추출 하거나 즉시 계산 하는 어댑터를 수정 될 수 있습니다. 에 대 한 자세한 내용은 `ListView` 어댑터 참조 [ListView](~/android/user-interface/layouts/list-view/index.md)합니다.

응용 프로그램을 빌드 및 실행합니다. 다음 스크린샷은 다음과 같습니다. 앱을 장치에서 실행할 때 표시 되는 방식을 예로

[![최종 스크린 샷](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

첫 번째 단계는 새 Xamarin.Android 프로젝트를 만드는 것입니다.

하 고 Mac 용 Visual Studio 시작 **새 프로젝트...** . 선택 된 **Android 앱** 템플릿과 클릭 **다음**:

[![비어 있는 android 앱](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

새 앱 이름을 **DesignerWalkthrough**합니다. 아래 **대상 플랫폼**를 선택 **최신 및 최고의** 클릭 **다음**:

[![앱 이름](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

다음 대화 상자 화면에서 클릭 **만들기**합니다.

### <a name="adding-a-layout"></a>레이아웃을 추가합니다.

다음 단계를 만드는 것을 **LinearLayout** 를 보유 하는 사용자 인터페이스 요소입니다.

Mac 용 Visual Studio에서 마우스 오른쪽 단추로 클릭 **리소스/레이아웃** 에 **솔루션** 패딩 선택한 **추가 > 새 파일...** . 에 **새 파일** 대화 상자에서 **Android > 레이아웃**합니다. 파일 이름을 **list_item** 누릅니다 **새로 만들기**:

[![새 레이아웃](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

이 파일이 추가 되 면 새 **list_item** 레이아웃에 표시 되는 **디자인 화면** (메시지가 표시 되 면 *이 프로젝트는 성공적으로 컴파일되지 않은 리소스를 포함 합니다. 렌더링에 영향을 줄 수 있습니다*, 클릭 **빌드 > 모두 빌드** 프로젝트를 빌드하려면):

[![디자이너 뷰](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

클릭 합니다 **원본** 이 레이아웃에 대 한 XML 소스를 보려면 디자이너의 아래쪽에 있는 탭입니다. 클릭할 때 합니다 **문서 개요** 탭 오른쪽의 레이아웃에 현재 단일 포함 표시 **LinearLayout** 위젯:

[![XML 디자이너](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

다음 단계는 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 것입니다.

### <a name="creating-the-list-item-user-interface"></a>목록 항목 사용자 인터페이스 만들기

클릭 합니다 **디자이너** 돌아가려면 화면 아래쪽의 탭을 **디자이너 화면**합니다. 에 **도구 상자** 창 오른쪽으로 스크롤하여 합니다 **이미지 및 미디어** 섹션을 찾습니다 `ImageView`:

[![ImageView 찾기](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

또는 입력할 수 있습니다 *ImageView* 찾으려고 검색 표시줄에는 `ImageView`:

[![ImageView 검색](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

이 드래그 `ImageView` 에 **디자인 화면** (이 `ImageView` 색 견본 색 브라우저 앱에서 표시 하는 데 사용할):

[![캔버스에서 ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

를 끌어를 `LinearLayout (Vertical)` 에서 위젯 합니다 **도구 상자** 에 **디자인 화면**합니다. 파란색 윤곽선 추가한 경계 나타냅니다 `LinearLayout`합니다. 합니다 **문서 개요** 의 자식 임을 `LinearLayout`아래에 있는 `imageView1 (ImageView)`:

[![파란색 윤곽선](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

선택 하는 경우는 `ImageView` 디자이너에서를 이동 하는 파란색 윤곽선을 `ImageView`입니다. 또한, 선택 영역을 이동할 `imageView1 (ImageView)` 에 **문서 개요**:

[![ImageView 선택](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

를 끌어를 `Text (Large)` 에서 위젯 합니다 **도구 상자** 에 새로 추가 된 `LinearLayout`합니다. 에 마우스를 끌면 있음을 합니다 **디자인 화면**, 새 위젯 삽입 될 위치 강조 표시 합니다.
합니다 `Text (Large)` 위젯 내에서 있어야 `linearLayout1` 다음과 같이 합니다.

[![큰 텍스트 위젯 추가](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

그런 다음 추가 `Text (Small)` 위젯 아래는 `Text (Large)` 위젯입니다. 이 시점에서 **디자인 화면** 은 다음 스크린샷과 유사 해야 합니다.

[![작은 텍스트 위젯 추가](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

하는 경우 두 `textView` 위젯은 inside `linearLayout1`를 끌어올 수 있습니다 `linearLayout1` 에 **문서 개요** 와 이전 스크린샷에 표시 된 대로 나타나도록 위치를 ( 아래에들여쓰기`linearLayout1`).


### <a name="arranging-the-user-interface"></a>사용자 인터페이스를 정렬합니다.

UI 표시를 수정 하려면 다음 단계는 합니다 `ImageView` 왼쪽에서 두 개의 `TextView` 위젯 오른쪽의 누적는 `ImageView`합니다.

1.  사용 하 여는 `ImageView` 클릭을 선택 합니다 **속성** 탭 합니다.

2.  바로 아래 합니다 **속성** 탭을 클릭 **레이아웃**합니다.

3.  아래로 스크롤하여 **ViewGroup** 변경 된 `Width` 로 설정 `wrap_content`:

[![줄 바꿈 콘텐츠 집합](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

변경 하는 또 다른 방법은 합니다 `Width` 설정은 해당 너비 설정을 전환 하려면 위젯의 오른쪽에 있는 삼각형을 클릭 하는 것 `wrap_content`:

[![끌어서 너비를 설정 하려면](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

반환 삼각형을 다시 클릭 하 여 `Width` 로 설정 `match_parent`합니다. 다음으로 이동 합니다 **문서 개요** 창과 선택 루트 `LinearLayout`:

[![LinearLayout 루트를 선택 합니다.](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

루트를 가진 `LinearLayout` 돌아가기 옵션을 선택 합니다 **속성** 탭을 클릭 **위젯**합니다. 변경 된 `Orientation` 설정을 `horizontal` 아래와 같이 합니다. 이 시점에서 **디자인 화면** 은 다음 스크린샷과 유사 해야 합니다. 다음에 유의 합니다 `TextView` 위젯을 오른쪽으로 이동 되었습니다는 `ImageView`:

[![가로 방향을 선택합니다](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>간격 수정

위젯 간에 더 많은 공간을 제공 하려면 UI의 안쪽 여백 및 여백 설정을 수정 하려면 다음 단계가입니다. 선택 합니다 `ImageView` 을 클릭 합니다 **레이아웃** 탭 **속성**. 변경 합니다 `Min Width` 에 `50dp`, `Min Height` 에 `70dp`, 및 `Padding` 를 `10dp`입니다.
모든 면 주위에 안쪽 여백을 적용 됩니다는 `ImageView` 것을 세로로 elongates 및:

[![요소의 안쪽 여백 설정](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

위쪽, 오른쪽, 아래쪽 및 왼쪽 안쪽 여백 설정이 설정할 수 있습니다 독립적으로 값을 입력 하 여 합니다 `Top`, `Right`를 `Bottom`, 및 `Left` 필드를 각각 패딩 합니다. 예를 들어 설정 합니다 `Left` 안쪽 여백 값을 `5dp` 및 `Top`, `Right`, 및 `Bottom` 안쪽 여백 값을 `10dp`입니다. `Padding` 설정이 이러한 값의 쉼표로 구분 된 목록으로 변경 합니다.

[![사용자 지정 패딩 설정](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

그런 다음의 위치를 조정 합니다 `LinearLayout` 두 가지를 포함 하는 위젯 `TextView` 위젯입니다. 에 **문서 개요**, 선택 `linearLayout1`합니다. 에 **속성** 창 합니다 **레이아웃** 탭 합니다. 아래로 스크롤하여를 **ViewGroup** 섹션 및 설정 합니다 `Left`, `Top`, `Right`, 및 `Bottom` 에 여백 `5dp`, `5dp`, `0dp`, 및 `5dp` 각각.

[![여백 설정](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>기본 이미지를 제거합니다.

때문에 `ImageView` 색을 표시 하는 데 사용 되는 템플릿에 의해 추가 된 기본 이미지 소스를 제거 하려면 다음 단계는 (이미지)를 대신 합니다.

1.  `ImageView`를 선택합니다.

2.  클릭 합니다 **위젯** 탭에서 **속성**합니다.

3.  Clear는 `Src` 비어 있도록 설정 합니다.

[![ImageView src 설정의 선택을 취소합니다](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

이 제거 `android:src="@android:drawable/ic_menu_gallery"` 소스 XML에서에서 해당 `ImageView`합니다.

### <a name="adding-a-listview-container"></a>ListView 컨테이너 추가

이제는 **list_item** 레이아웃 정의 된, 다음 단계를 추가 하는 것을 `ListView` 기본 레이아웃으로 합니다. 이렇게 `ListView` 목록이 포함 됩니다 **list_item**합니다. 

에 **솔루션 탐색기**오픈 **Resources/layout/Main.axml**합니다.
클릭 된 `Button` 위젯 (있는 경우)를 삭제 합니다. 에 **도구 상자**를 찾습니다는 `ListView` 위젯 끕니다를 **디자인 화면**합니다.
`ListView` 디자이너에서 비어 있게 됩니다 선택 될 때 해당 테두리를 간략하게 설명 하는 파란색 선은 제외 하 고 있습니다. 볼 수 있습니다는 **문서 개요** 되었는지 확인 하는 **ListView** 가 올바르게 추가:

[![새 ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

기본적으로 `ListView` 같습니다는 `Id` 값 `@+id/listView1`합니다.
하는 동안 `listView1` 가 선택 된 상태로 합니다 **문서 개요**엽니다는 **속성** 창 클릭 **정렬 기준**, 선택한 **범주**.
열기 **Main**를 찾습니다는 **Id** 속성 값을 변경 하 고 `@+id/myListView`:

[![MyListView id의 이름을](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

이 시점에서 사용자 인터페이스를 사용할 준비가 되었습니다.

### <a name="running-the-application"></a>응용 프로그램 실행

오픈 **MainActivity.cs** 및 해당 코드를 다음으로 바꿉니다.

```csharp
using Android.App;
using Android.Widget;
using Android.Views;
using Android.OS;
using System.Collections.Generic;

namespace DesignerWalkthrough
{
    [Activity(Label = "@string/app_name", MainLauncher = true)]
    public class MainActivity : Activity
    {
        List<ColorItem> colorItems = new List<ColorItem>();
        ListView listView;

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set our view from the "main" layout resource
            SetContentView(Resource.Layout.Main);
            listView = FindViewById<ListView>(Resource.Id.myListView);

            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.DarkRed,
                ColorName = "Dark Red",
                Code = "8B0000"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.SlateBlue,
                ColorName = "Slate Blue",
                Code = "6A5ACD"
            });
            colorItems.Add(new ColorItem()
            {
                Color = Android.Graphics.Color.ForestGreen,
                ColorName = "Forest Green",
                Code = "228B22"
            });

            listView.Adapter = new ColorAdapter(this, colorItems);
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
        public override View GetView(int position, View convertView, ViewGroup parent)
        {
            var item = items[position];

            View view = convertView;
            if (view == null) // no view to re-use, create new
                view = context.LayoutInflater.Inflate(Resource.Layout.list_item, null);
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

이 코드에서는 사용자 지정 `ListView` 어댑터 색 정보를 로드 하 고 방금 만든 UI에이 데이터를 표시 합니다. 을 유지 하기 위해이 예제에서는 짧은 색 정보를 목록에 하드 코딩 되었지만를 데이터 원본에서 색 정보를 추출 하거나 즉시 계산 하는 어댑터를 수정 될 수 있습니다. 에 대 한 자세한 내용은 `ListView` 어댑터 참조 [ListView](~/android/user-interface/layouts/list-view/index.md)합니다.

응용 프로그램을 빌드 및 실행합니다. 다음 스크린샷은 다음과 같습니다. 앱을 장치에서 실행할 때 표시 되는 방식을 예로

[![최종 스크린 샷](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>요약

이 문서에서는 Visual Studio에서 Xamarin.Android 디자이너를 사용 하 여 기본적인 앱을 위한 사용자 인터페이스를 만드는 과정을 단계별로 안내 합니다.
목록에서 단일 항목에 대 한 인터페이스를 만드는 방법을 보여 줍니다 및 위젯을 추가 하 고 시각적으로 레이아웃 하는 방법 설명 된 것입니다.
또한 리소스를 할당 한 다음 해당 위젯을 다양 한 속성을 설정 하는 방법을 설명 했습니다.
