---
title: Xamarin. Android Designer 사용
description: 이 문서는 Xamarin. Android Designer의 연습입니다. 작은 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스는 전적으로 디자이너에 생성 됩니다.
ms.prod: xamarin
ms.assetid: 70FF2F9A-71BD-317E-C881-A44D82DF1BD8
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 9387b44419af87785d45a25ab254d3361a5615a3
ms.sourcegitcommit: c75c1d2132a4f46a7b38e454d5f24705165026bd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68485919"
---
# <a name="using-the-xamarinandroid-designer"></a>Xamarin. Android Designer 사용

_이 문서는 Xamarin. Android Designer의 연습입니다. 작은 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 방법을 보여 줍니다. 이 사용자 인터페이스는 전적으로 디자이너에 생성 됩니다._


## <a name="overview"></a>개요

코드를 작성 하 여 프로그래밍 방식으로 또는 XML 파일을 사용 하 여 Android 사용자 인터페이스를 선언적으로 만들 수 있습니다. 개발자는 Android Designer Xamarin.ios를 사용 하 여 XML 파일을 직접 편집 하지 않고도 선언적 레이아웃을 시각적으로 만들고 수정할 수 있습니다. 또한 디자이너는 장치 또는 에뮬레이터에 응용 프로그램을 다시 배포할 필요 없이 개발자가 UI 변경을 평가할 수 있도록 실시간 피드백을 제공 합니다. 이러한 디자이너 기능을 통해 Android UI development 크게의 속도를 높일 수 있습니다.
이 문서에서는 Android Designer를 사용 하 여 사용자 인터페이스를 시각적으로 만드는 방법을 보여 줍니다.

> [!TIP]
> Visual Studio의 최신 릴리스에서는 Android Designer 내에서 .xml 파일을 여는 것을 지원 합니다.
>
> . Axml 및 .xml 파일은 모두 Android Designer에서 지원 됩니다.

## <a name="walkthrough"></a>연습

이 연습의 목적은 Android Designer를 사용 하 여 예제 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 것입니다. 색 브라우저 앱은 색, 이름 및 RGB 값의 목록을 제공 합니다. 이러한 위젯을 시각적으로 레이아웃 하는 방법 뿐만 아니라 **Design Surface** 에 위젯을 추가 하는 방법을 알아봅니다. 그런 다음 **Design Surface** 에서 또는 디자이너의 **속성** 창을 사용 하 여 대화형으로 위젯을 수정 하는 방법에 대해 알아봅니다. 마지막으로 장치 또는 에뮬레이터에서 앱을 실행할 때 디자인의 모양을 확인할 수 있습니다.


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

첫 번째 단계는 새 Xamarin Android 프로젝트를 만드는 것입니다. Visual Studio를 시작 하 고, **새 프로젝트 ...** 를 클릭 하 고, **visual\# C > android > android 앱 (Xamarin)** 템플릿을 선택 합니다.
새 앱 **Designerwalkthrough** 이름을로, **확인**을 클릭 합니다.

[![Android 빈 앱](designer-walkthrough-images/vs/01-android-app-w158-sml.png)](designer-walkthrough-images/vs/01-android-app-w158.png#lightbox)

**새 Android 앱** 대화 상자에서 비어 있는 **앱** 을 선택 하 고 **확인**을 클릭 합니다.

[![Android 빈 앱 템플릿 선택](designer-walkthrough-images/vs/02-blank-app-w158-sml.png)](designer-walkthrough-images/vs/02-blank-app-w158.png#lightbox)


### <a name="adding-a-layout"></a>레이아웃 추가

다음 단계는 사용자 인터페이스 요소를 보유 하는 **LinearLayout** 을 만드는 것입니다. **솔루션 탐색기** 에서 **리소스/레이아웃** 을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목**...을 선택 합니다. **새 항목 추가** 대화 상자에서 **Android 레이아웃**을 선택 합니다. 파일 이름을 **list_item** 로 하 고 **추가**를 클릭 합니다.

[![새 레이아웃](designer-walkthrough-images/vs/03-new-layout-w158-sml.png)](designer-walkthrough-images/vs/03-new-layout-w158.png#lightbox)

새 **list_item** 레이아웃이 디자이너에 표시 됩니다. 왼쪽 창에는 **list_item** 의 &ndash; 두 창이 *Design Surface* 표시 되 고 해당 XML 소스가 오른쪽 창에 표시 되는 것을 볼 수 있습니다. 두 창 사이에 있는 **창 바꾸기** 아이콘을 클릭 하 여 **Design Surface** 및 **소스** 창의 위치를 바꿀 수 있습니다.

[![디자이너 뷰](designer-walkthrough-images/vs/04-designer-view-w158-sml.png)](designer-walkthrough-images/vs/04-designer-view-w158.png#lightbox)

**보기** 메뉴에서 **다른 창 > 문서 개요** 를 클릭 하 여 **문서 개요**를 엽니다. **문서 개요** 는 레이아웃에 현재 단일 **LinearLayout** 위젯이 포함 되어 있음을 보여 줍니다.

[![문서 개요](designer-walkthrough-images/vs/06-document-outline-w158-sml.png)](designer-walkthrough-images/vs/06-document-outline-w158.png#lightbox)

다음 단계는이 `LinearLayout`내에서 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 것입니다.

### <a name="creating-the-list-item-user-interface"></a>목록 항목 사용자 인터페이스 만들기

**도구 상자** 창이 표시 되지 않으면 왼쪽에 있는 **도구 상자** 탭을 클릭 합니다. **도구 상자**에서 **이미지 & 미디어** 섹션까지 아래로 스크롤하고를 찾을 `ImageView`때까지 아래로 스크롤합니다.

[![ImageView 찾기](designer-walkthrough-images/vs/07-locate-imageview-w158-sml.png)](designer-walkthrough-images/vs/07-locate-imageview-w158.png#lightbox)

또는 검색 창에 *Imageview* 를 입력 하 여 `ImageView`다음을 찾을 수 있습니다.

[![ImageView 검색](designer-walkthrough-images/vs/08-imageview-search-w158-sml.png)](designer-walkthrough-images/vs/08-imageview-search-w158.png#lightbox)

이 `ImageView` 를 Design Surface로 끌어 옵니다 `ImageView` (색 브라우저 앱에서 색 견본을 표시 하는 데 사용 됨).

[![캔버스의 ImageView](designer-walkthrough-images/vs/09-imageview-on-canvas-w158-sml.png)](designer-walkthrough-images/vs/09-imageview-on-canvas-w158.png#lightbox)

그런 다음 `LinearLayout (Vertical)` **도구 상자** 의 위젯을 디자이너로 끌어 옵니다. 파란색 윤곽선은 추가 `LinearLayout`된의 경계를 나타냅니다. **문서 개요** 는 아래 `LinearLayout` `imageView1 (ImageView)`에 있는의 자식 임을 보여 줍니다.

[![파란색 윤곽선](designer-walkthrough-images/vs/10-blue-outline-w158-sml.png)](designer-walkthrough-images/vs/10-blue-outline-w158.png#lightbox)

디자이너에서를 `ImageView` 선택 하면 파란색 윤곽선이로 이동 하 여로 `ImageView`이동 합니다. 또한 **문서 개요**에서 선택 항목이로 `imageView1 (ImageView)` 이동 합니다.

[![ImageView 선택](designer-walkthrough-images/vs/11-select-imageview-w158-sml.png)](designer-walkthrough-images/vs/11-select-imageview-w158.png#lightbox)

그런 다음 `Text (Large)` **도구 상자** 의 위젯을 새로 추가 `LinearLayout`된로 끌어 옵니다. 디자이너는 녹색 강조 표시를 사용 하 여 새 위젯이 삽입 되는 위치를 표시 합니다.

[![녹색 강조 표시](designer-walkthrough-images/vs/12-green-highlight-w158-sml.png)](designer-walkthrough-images/vs/12-green-highlight-w158.png#lightbox)

다음으로 `Text (Large)` 위젯 아래 `Text (Small)` 에 위젯을 추가 합니다.

[![작은 텍스트 위젯 추가](designer-walkthrough-images/vs/13-add-small-text-w158-sml.png)](designer-walkthrough-images/vs/13-add-small-text-w158.png#lightbox)

이 시점에서 디자이너 화면은 다음 스크린샷과 유사 합니다.

[![디자이너 레이아웃](designer-walkthrough-images/vs/14-raw-layout-w158-sml.png)](designer-walkthrough-images/vs/14-raw-layout-w158.png#lightbox)

두 `textView` 위젯이 내부가 `linearLayout1`아니면 **문서 개요** 에서로 `linearLayout1` `linearLayout1` 끌어 오고 앞의 스크린샷에 표시 된 것 처럼 나타나도록 할 수 있습니다 (아래 들여쓰기).


### <a name="arranging-the-user-interface"></a>사용자 인터페이스 정렬

다음 단계는 왼쪽 `ImageView` 에를 표시 하도록 UI를 수정 하 고 오른쪽 `ImageView`에 두 개의 `TextView` 위젯을 누적 하는 것입니다.

1.  `ImageView`를 선택합니다.

2.  **속성 창**검색 상자에 *너비* 를 입력 하 고 **레이아웃 너비**를 찾습니다.

3.  **레이아웃 너비** 설정을 `wrap_content`다음과 같이 변경 합니다.

![콘텐츠 래핑 설정](designer-walkthrough-images/vs/15-wrap-content-w158.png)

`Width` 설정을 변경 하는 또 다른 방법은 위젯의 오른쪽에 있는 삼각형을 클릭 하 여 너비 `wrap_content`설정을 다음과 같이 설정/해제 하는 것입니다.

![너비를 설정 하려면 끌어 옵니다.](designer-walkthrough-images/vs/15b-width-arrow-w158.png)

삼각형을 다시 클릭 하면 `Width` 설정이로 `match_parent`반환 됩니다. 다음으로 **문서 개요** 창으로 이동 하 여 루트 `LinearLayout`를 선택 합니다.

[![Root LinearLayout 선택](designer-walkthrough-images/vs/16-root-linearlayout-w158-sml.png)](designer-walkthrough-images/vs/16-root-linearlayout-w158.png#lightbox)

루트 `LinearLayout` 를 선택 하 고 **속성** 창으로 돌아간 다음 검색 상자에 *방향* 을 입력 하 고 **방향** 설정을 찾습니다. 방향`horizontal`변경:

![가로 방향 선택](designer-walkthrough-images/vs/17-horizontal-orientation-w158.png)

이 시점에서 디자이너 화면은 다음 스크린샷과 유사 합니다.
`TextView` 위젯은 의오른쪽으로이동된것을`ImageView`볼 수 있습니다.

[![디자이너 레이아웃](designer-walkthrough-images/vs/18-designer-layout-w158-sml.png)](designer-walkthrough-images/vs/18-designer-layout-w158.png#lightbox)

### <a name="modifying-the-spacing"></a>간격 수정

다음 단계는 UI의 안쪽 여백 및 여백 설정을 수정 하 여 위젯 사이에 더 많은 공간을 제공 하는 것입니다. 디자인 화면 `ImageView` 에서를 선택 합니다. **속성** 창의 검색 상자에을 `min` 입력 합니다. `70dp` **최소 높이** 및 `50dp` **최소 너비**를 입력 합니다.

[![높이 및 너비 설정](designer-walkthrough-images/vs/18b-set-height-width-sml.png)](designer-walkthrough-images/vs/18b-set-height-width.png#lightbox)

**속성** 창에서 검색 상자에 `padding` 를 입력 하 고 **안쪽 여백**으로를 입력 `10dp` 합니다. 이러한 `minHeight`및 설정은`padding`의 모든 면 주위에 안쪽 여백을 추가 하 `ImageView` 고 세로로 elongate. `minWidth` 이러한 값을 입력 하면 레이아웃 XML이 변경 됩니다.

[![안쪽 여백 설정](designer-walkthrough-images/vs/19-padding-widths-w158-sml.png)](designer-walkthrough-images/vs/19-padding-widths-w158.png#lightbox)

아래쪽, 왼쪽, 오른쪽 및 위쪽 패딩 설정은 각각 **안쪽 여백 아래쪽**, **안쪽 여백 왼쪽**, **안쪽 여백 오른쪽**및 **안쪽 여백** 필드에 값을 입력 하 여 개별적으로 설정할 수 있습니다.
예를 들어 **왼쪽 안쪽 여백** 필드를로 `5dp` 설정 하 고 **안쪽 여백 아래쪽**안쪽 여백, **오른쪽 안쪽**여백 및 `10dp` **안쪽** 여백 필드를로 설정 합니다.

[![사용자 지정 패딩 설정](designer-walkthrough-images/vs/20-custom-padding-w158-sml.png)](designer-walkthrough-images/vs/20-custom-padding-w158.png#lightbox)

다음으로 두 `TextView` 위젯을 포함 하는 `LinearLayout` 위젯의 위치를 조정 합니다. **문서 개요**에서를 선택 `linearLayout1`합니다. **속성** 창의 검색 상자에를 `margin` 입력 합니다. **레이아웃 여백 아래쪽**, **레이아웃 여백 왼쪽**및 **레이아웃 여백을 위쪽** 으로 `5dp`설정 합니다. **레이아웃 여백을 오른쪽** 으로 설정 `0dp`합니다.

[![여백 설정](designer-walkthrough-images/vs/21-margins-w158-sml.png)](designer-walkthrough-images/vs/21-margins-w158.png#lightbox)

### <a name="removing-the-default-image"></a>기본 이미지 제거

는 `ImageView` 이미지 대신 색을 표시 하는 데 사용 되기 때문에 다음 단계는 템플릿에 의해 추가 된 기본 이미지 원본을 제거 하는 것입니다.

1.  `ImageView` **디자이너 화면**에서을 선택 합니다.

2.  **속성**의 검색 상자에 *src* 를 입력 합니다.

3.  **Src** 속성 설정 오른쪽의 작은 사각형을 클릭 하 고 **다시 설정**을 선택 합니다.

[![ImageView src 설정 지우기](designer-walkthrough-images/vs/22-clear-img-src-w158-sml.png)](designer-walkthrough-images/vs/22-clear-img-src-w158.png#lightbox)

그러면 소스 `android:src="@android:drawable/ic_menu_gallery"` XML에서 해당 `ImageView`이 제거 됩니다.

### <a name="adding-a-listview-container"></a>ListView 컨테이너 추가

이제 **list_item** 레이아웃이 정의 되었으므로 다음 단계는를 `ListView` 주 레이아웃에 추가 하는 것입니다. **List_item 목록이**포함 `ListView` 됩니다. 

**솔루션 탐색기**에서 **Resources/layout/activity_main**을 엽니다. **도구 상자**에서 `ListView` 위젯을 찾아 **Design Surface**끌어 옵니다. 선택 `ListView` 시 테두리를 윤곽선으로 표시할 때를 제외 하 고는 디자이너의가 비어 있습니다. **문서 개요** 를 보고 **ListView** 가 올바르게 추가 되었는지 확인할 수 있습니다.

[![새 ListView](designer-walkthrough-images/vs/23-new-listview-w158-sml.png)](designer-walkthrough-images/vs/23-new-listview-w158.png#lightbox)

기본적으로에 `ListView` 는 `Id` 값 `@+id/listView1`이 지정 됩니다.
`listView1` **문서 개요**에서를 선택한 상태에서 **속성** 창을 열고 **정렬 기준**을 클릭 한 다음 **범주**를 선택 합니다.
**Main**을 열고 **Id** 속성을 찾은 다음 해당 값을로 `@+id/myListView`변경 합니다.

[![Id 이름을 myListView로 바꾸기](designer-walkthrough-images/vs/24-change-id-w158-sml.png)](designer-walkthrough-images/vs/24-change-id-w158.png#lightbox)

이 시점에서 사용자 인터페이스를 사용할 준비가 된 것입니다.

### <a name="running-the-application"></a>애플리케이션 실행

**MainActivity.cs** 를 열고 해당 코드를 다음으로 바꿉니다.

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

이 코드는 사용자 지정 `ListView` 어댑터를 사용 하 여 색 정보를 로드 하 고 방금 만든 UI에이 데이터를 표시 합니다. 이 예를 짧게 유지 하기 위해 색 정보는 목록에 하드 코딩 되지만 어댑터는 데이터 원본에서 색 정보를 추출 하거나 즉석에서 계산 하도록 수정할 수 있습니다. 어댑터에 대 한 `ListView` 자세한 내용은 [ListView](~/android/user-interface/layouts/list-view/index.md)를 참조 하세요.

애플리케이션을 빌드 및 실행합니다. 다음 스크린샷은 장치에서 실행 될 때 앱이 표시 되는 방법의 예입니다.

[![최종 스크린샷](designer-walkthrough-images/vs/25-final-screenshot-sml.png)](designer-walkthrough-images/vs/25-final-screenshot.png#lightbox)



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="creating-a-new-project"></a>새 프로젝트 만들기

첫 번째 단계는 새 Xamarin Android 프로젝트를 만드는 것입니다.

Mac용 Visual Studio를 시작 하 고 **새 프로젝트**...를 클릭 합니다. **Android 앱** 템플릿을 선택 하 고 **다음**을 클릭 합니다.

[![Android 빈 앱](designer-walkthrough-images/xs/01-android-app-m75-sml.png)](designer-walkthrough-images/xs/01-android-app-m75.png#lightbox)

새 앱 **Designerwalkthrough**이름을로 합니다. **대상 플랫폼**에서 **최신 및 가장** 크게를 선택 하 고 **다음**을 클릭 합니다.

[![앱 이름](designer-walkthrough-images/xs/02-designer-walkthrough-m75-sml.png)](designer-walkthrough-images/xs/02-designer-walkthrough-m75.png#lightbox)

다음 대화 상자 화면에서 **만들기**를 클릭 합니다.

### <a name="adding-a-layout"></a>레이아웃 추가

다음 단계는 사용자 인터페이스 요소를 보유 하는 **LinearLayout** 을 만드는 것입니다.

Mac용 Visual Studio에서 **Solution** Pad의 **리소스/레이아웃** 을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 파일**...을 선택 합니다. **새 파일** 대화 상자에서 **Android > 레이아웃**을 선택 합니다. 파일 이름을 **list_item** 로, **새로 만들기**를 클릭 합니다.

[![새 레이아웃](designer-walkthrough-images/xs/03-new-layout-m75-sml.png)](designer-walkthrough-images/xs/03-new-layout-m75.png#lightbox)

이 파일이 추가 되 면 새 **list_item** 레이아웃이 **Design Surface** 에 표시 됩니다. 메시지가 표시 되 면 *이 프로젝트에 성공적으로 컴파일되지 않은 리소스가 포함 되 고, 렌더링에 영향을 줄 수 있는*경우, **빌드를 클릭 > 모든 빌드를 빌드하여** 프로젝트를 빌드합니다.

[![디자이너 뷰](designer-walkthrough-images/xs/04-designer-view-m75-sml.png)](designer-walkthrough-images/xs/04-designer-view-m75.png#lightbox)

디자이너 아래쪽의 **원본** 탭을 클릭 하 여이 레이아웃에 대 한 XML 원본을 봅니다. 오른쪽의 **문서 개요** 탭을 클릭 하면 레이아웃에 현재 단일 **LinearLayout** 위젯이 포함 되어 있음을 보여 줍니다.

[![디자이너 XML](designer-walkthrough-images/xs/05-designer-xml-m75-sml.png)](designer-walkthrough-images/xs/05-designer-xml-m75.png#lightbox)

다음 단계는 색 브라우저 앱에 대 한 사용자 인터페이스를 만드는 것입니다.

### <a name="creating-the-list-item-user-interface"></a>목록 항목 사용자 인터페이스 만들기

**디자이너 화면**으로 돌아가려면 화면 아래쪽의 **디자이너** 탭을 클릭 합니다. 오른쪽의 **도구 상자** 창에서 **이미지 & 미디어** 섹션까지 아래로 스크롤하고 다음을 찾습니다 `ImageView`.

[![ImageView 찾기](designer-walkthrough-images/xs/06-locate-imageview-m75-sml.png)](designer-walkthrough-images/xs/06-locate-imageview-m75.png#lightbox)

또는 검색 창에 *Imageview* 를 입력 하 여 `ImageView`다음을 찾을 수 있습니다.

[![ImageView 검색](designer-walkthrough-images/xs/07-imageview-search-m75-sml.png)](designer-walkthrough-images/xs/07-imageview-search-m75.png#lightbox)

이 `ImageView` 를 **Design Surface로** 끌어 옵니다 ( 색브라우저앱에서색견본을표시하는데사용됨).`ImageView`

[![캔버스의 ImageView](designer-walkthrough-images/xs/08-imageview-on-canvas-m75-sml.png)](designer-walkthrough-images/xs/08-imageview-on-canvas-m75.png#lightbox)

그런 다음 `LinearLayout (Vertical)` **도구 상자** 의 위젯을 **Design Surface**로 끌어 옵니다. 파란색 윤곽선은 추가 `LinearLayout`된의 경계를 나타냅니다. **문서 개요** 는 아래 `LinearLayout` `imageView1 (ImageView)`에 있는의 자식 임을 보여 줍니다.

[![파란색 윤곽선](designer-walkthrough-images/xs/10-blue-outline-m75-sml.png)](designer-walkthrough-images/xs/10-blue-outline-m75.png#lightbox)

디자이너에서를 `ImageView` 선택 하면 파란색 윤곽선이로 이동 하 여로 `ImageView`이동 합니다. 또한 **문서 개요**에서 선택 항목이로 `imageView1 (ImageView)` 이동 합니다.

[![ImageView 선택](designer-walkthrough-images/xs/11-select-imageview-m75-sml.png)](designer-walkthrough-images/xs/11-select-imageview-m75.png#lightbox)

그런 다음 `Text (Large)` **도구 상자** 의 위젯을 새로 추가 `LinearLayout`된로 끌어 옵니다. 마우스를 **Design Surface**위로 끌면 새 위젯이 삽입 되는 위치가 강조 표시 됩니다.
위젯은 `Text (Large)` 다음과 같이 내 `linearLayout1` 에 위치 해야 합니다.

[![큰 텍스트 위젯 추가](designer-walkthrough-images/xs/12-green-highlight-m75-sml.png)](designer-walkthrough-images/xs/12-green-highlight-m75.png#lightbox)

다음으로 `Text (Large)` 위젯 아래 `Text (Small)` 에 위젯을 추가 합니다. 이 시점에서 **Design Surface** 는 다음 스크린샷에서와 비슷해야 합니다.

[![작은 텍스트 위젯 추가](designer-walkthrough-images/xs/13-add-small-text-m75-sml.png)](designer-walkthrough-images/xs/13-add-small-text-m75.png#lightbox)

두 `textView` 위젯이 내부가 `linearLayout1`아니면 **문서 개요** 에서로 `linearLayout1` `linearLayout1` 끌어 오고 앞의 스크린샷에 표시 된 대로 나타나도록 지정할 수 있습니다 (아래 들여쓰기).


### <a name="arranging-the-user-interface"></a>사용자 인터페이스 정렬

다음 단계는 왼쪽 `ImageView` 에를 표시 하도록 UI를 수정 하 고 오른쪽 `ImageView`에 두 개의 `TextView` 위젯을 누적 하는 것입니다.

1.  선택 된 상태에서 속성 탭을 클릭 합니다. `ImageView`

2.  **속성** 탭 바로 아래에서 **레이아웃**을 클릭 합니다.

3.  **ViewGroup** 까지 아래로 스크롤하고 설정을 `Width` `wrap_content`다음과 같이 변경 합니다.

[![콘텐츠 래핑 설정](designer-walkthrough-images/xs/15-wrap-content-m75-sml.png)](designer-walkthrough-images/xs/15-wrap-content-m75.png#lightbox)

`Width` 설정을 변경 하는 또 다른 방법은 위젯의 오른쪽에 있는 삼각형을 클릭 하 여 너비 `wrap_content`설정을 다음과 같이 설정/해제 하는 것입니다.

[![너비를 설정 하려면 끌어 옵니다.](designer-walkthrough-images/xs/16-width-arrow-m75-sml.png)](designer-walkthrough-images/xs/16-width-arrow-m75.png#lightbox)

삼각형을 다시 클릭 하면 `Width` 설정이로 `match_parent`반환 됩니다. 다음으로 **문서 개요** 창으로 이동 하 여 루트 `LinearLayout`를 선택 합니다.

[![Root LinearLayout 선택](designer-walkthrough-images/xs/17-root-linearlayout-m75-sml.png)](designer-walkthrough-images/xs/17-root-linearlayout-m75.png#lightbox)

루트 `LinearLayout` 를 선택한 상태에서 **속성** 탭으로 돌아가서 **위젯을**클릭 합니다. 아래와 같이 `horizontal`설정을로 변경 합니다. `Orientation` 이 시점에서 **Design Surface** 는 다음 스크린샷에서와 비슷해야 합니다. `TextView` 위젯은 의오른쪽으로이동된것을`ImageView`볼 수 있습니다.

[![가로 방향 선택](designer-walkthrough-images/xs/18-horizontal-orientation-m75-sml.png)](designer-walkthrough-images/xs/18-horizontal-orientation-m75.png#lightbox)


### <a name="modifying-the-spacing"></a>간격 수정

다음 단계는 위젯 사이에 더 많은 공간을 제공 하도록 UI의 안쪽 여백 및 여백 설정을 수정 하는 것입니다. 을 `ImageView` 선택 하 고 **속성**아래에서 **레이아웃** 탭을 클릭 합니다. `Min Width` 을 로,`Min Height` 를로 ,`70dp` 를`10dp`로변경 합니다. `Padding` `50dp`
이렇게 하면 `ImageView` 의 모든 면에 안쪽 여백이 적용 되 고 세로로 elongates 됩니다.

[![안쪽 여백 설정](designer-walkthrough-images/xs/20-padding-widths-m75-sml.png)](designer-walkthrough-images/xs/20-padding-widths-m75.png#lightbox)

위쪽, 오른쪽, 아래쪽 및 왼쪽 안쪽 여백 설정은 `Top`각각, `Right`, `Bottom`및 `Left` 안쪽 여백 필드에 값을 입력 하 여 개별적으로 설정할 수 있습니다. 예를 들어 `Left` 안쪽 여백 값을로 `5dp` 설정 하 `Top`고 `Right`,, `Bottom` 및 안쪽 여백 `10dp`값을로 설정 합니다. `Padding` 설정은 다음 값의 쉼표로 구분 된 목록으로 변경 됩니다.

[![사용자 지정 패딩 설정](designer-walkthrough-images/xs/21-custom-padding-m75-sml.png)](designer-walkthrough-images/xs/21-custom-padding-m75.png#lightbox)

다음으로 두 `TextView` 위젯을 포함 하는 `LinearLayout` 위젯의 위치를 조정 합니다. **문서 개요**에서를 선택 `linearLayout1`합니다. **속성** 창에서 **레이아웃** 탭을 선택 합니다. **ViewGroup** 섹션 `Left`까지 아래로 스크롤하고,, 및 `Bottom` 여백을 각각 `Top`, `Right`, 및 `5dp` `0dp` `5dp` 로`5dp` 설정 합니다.

[![여백 설정](designer-walkthrough-images/xs/22-margins-m75-sml.png)](designer-walkthrough-images/xs/22-margins-m75.png#lightbox)

### <a name="removing-the-default-image"></a>기본 이미지 제거

는 `ImageView` 이미지 대신 색을 표시 하는 데 사용 되기 때문에 다음 단계는 템플릿에 의해 추가 된 기본 이미지 원본을 제거 하는 것입니다.

1.  `ImageView`를 선택합니다.

2.  **속성**에서 **위젯** 탭을 클릭 합니다.

3.  비어 있게 `Src` 설정의 선택을 취소 합니다.

[![ImageView src 설정 지우기](designer-walkthrough-images/xs/23-clear-src-m75-sml.png)](designer-walkthrough-images/xs/23-clear-src-m75.png#lightbox)

그러면 소스 `android:src="@android:drawable/ic_menu_gallery"` XML에서 해당 `ImageView`이 제거 됩니다.

### <a name="adding-a-listview-container"></a>ListView 컨테이너 추가

이제 **list_item** 레이아웃이 정의 되었으므로 다음 단계는를 `ListView` 주 레이아웃에 추가 하는 것입니다. **List_item 목록이**포함 `ListView` 됩니다. 

**솔루션 탐색기**에서 **리소스/레이아웃/기본. axml**을 엽니다.
`Button` 위젯 (있는 경우)을 클릭 하 고 삭제 합니다. **도구 상자**에서 `ListView` 위젯을 찾아 **Design Surface**끌어 옵니다.
선택 `ListView` 시 테두리를 윤곽선으로 표시할 때를 제외 하 고는 디자이너의가 비어 있습니다. **문서 개요** 를 보고 **ListView** 가 올바르게 추가 되었는지 확인할 수 있습니다.

[![새 ListView](designer-walkthrough-images/xs/24-new-listview-m75-sml.png)](designer-walkthrough-images/xs/24-new-listview-m75.png#lightbox)

기본적으로에 `ListView` 는 `Id` 값 `@+id/listView1`이 지정 됩니다.
`listView1` **문서 개요**에서를 선택한 상태에서 **속성** 창을 열고 **정렬 기준**을 클릭 한 다음 **범주**를 선택 합니다.
**Main**을 열고 **Id** 속성을 찾은 다음 해당 값을로 `@+id/myListView`변경 합니다.

[![Id 이름을 myListView로 바꾸기](designer-walkthrough-images/xs/25-change-id-m75-sml.png)](designer-walkthrough-images/xs/25-change-id-m75.png#lightbox)

이 시점에서 사용자 인터페이스를 사용할 준비가 된 것입니다.

### <a name="running-the-application"></a>애플리케이션 실행

**MainActivity.cs** 를 열고 해당 코드를 다음으로 바꿉니다.

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

이 코드는 사용자 지정 `ListView` 어댑터를 사용 하 여 색 정보를 로드 하 고 방금 만든 UI에이 데이터를 표시 합니다. 이 예를 짧게 유지 하기 위해 색 정보는 목록에 하드 코딩 되지만 어댑터는 데이터 원본에서 색 정보를 추출 하거나 즉석에서 계산 하도록 수정할 수 있습니다. 어댑터에 대 한 `ListView` 자세한 내용은 [ListView](~/android/user-interface/layouts/list-view/index.md)를 참조 하세요.

애플리케이션을 빌드 및 실행합니다. 다음 스크린샷은 장치에서 실행 될 때 앱이 표시 되는 방법의 예입니다.

[![최종 스크린샷](designer-walkthrough-images/xs/26-final-screenshot-sml.png)](designer-walkthrough-images/xs/26-final-screenshot.png#lightbox)

-----


## <a name="summary"></a>요약

이 문서에서는 Visual Studio에서 Android Designer를 사용 하 여 기본 앱에 대 한 사용자 인터페이스를 만드는 과정을 살펴보았습니다.
목록에서 단일 항목에 대 한 인터페이스를 만드는 방법을 보여 주고 위젯을 추가 하 고 시각적으로 레이아웃 하는 방법을 보여 주었습니다.
또한 리소스를 할당 하 고 해당 위젯에 대 한 다양 한 속성을 설정 하는 방법도 설명 했습니다.
