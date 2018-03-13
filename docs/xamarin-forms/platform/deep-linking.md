---
title: "응용 프로그램 인덱싱 및 직접 링크"
description: "응용 프로그램 인덱싱을 그렇지 않은 경우 무시 될 후 몇 가지 사용 하 여 검색 결과에 표시 하 여 관련 상태를 유지 하는 응용 프로그램 수 있습니다. 직접 링크 수 딥 링크에서 참조 하는 페이지로 이동 하 여 일반적으로 응용 프로그램 데이터를 포함 하는 검색 결과에 응답할 수 있습니다. 이 문서에서는 응용 프로그램 인덱싱을 사용 하는 방법과 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 하는 직접 링크를 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 38d3b6da0dd33e038f2d50209280f2983faf6013
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="application-indexing-and-deep-linking"></a>응용 프로그램 인덱싱 및 직접 링크

_응용 프로그램 인덱싱을 그렇지 않은 경우 무시 될 후 몇 가지 사용 하 여 검색 결과에 표시 하 여 관련 상태를 유지 하는 응용 프로그램 수 있습니다. 직접 링크 수 딥 링크에서 참조 하는 페이지로 이동 하 여 일반적으로 응용 프로그램 데이터를 포함 하는 검색 결과에 응답할 수 있습니다. 이 문서에서는 응용 프로그램 인덱싱을 사용 하는 방법과 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 하는 직접 링크를 보여줍니다._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**심층 Xamarin.Forms 및 Azure에 의해 링크 [Xamarin 대학](https://university.xamarin.com/)**


Xamarin.Forms 응용 프로그램 인덱싱 및 직접 링크 응용 프로그램 인덱싱을 응용 프로그램을 통해 사용자가 탐색에 대 한 메타 데이터 게시에 대 한 API를 제공 합니다. 스포트라이트 검색, Google 검색 또는 웹 검색에 대 한 인덱싱된 콘텐츠가 검색 다음 수 있습니다. 딥 링크를 포함 하는 검색 결과에 누르기 딥 링크에서 참조 하는 페이지를 탐색 하는 데 일반적으로 및 응용 프로그램에서 처리 될 수 있는 이벤트를 발생 합니다.

샘플 응용 프로그램에서는 다음 스크린샷에서 같이 로컬 SQLite 데이터베이스에 데이터가 저장 된 할 일 목록 응용 프로그램을 보여 줍니다.

![](deep-linking-images/screenshots.png "TodoList 응용 프로그램")

각 `TodoItem` 인스턴스는 사용자가 만든 인덱싱됩니다. 다음 플랫폼 관련 검색 응용 프로그램에서 인덱싱된 데이터를 찾는 데 사용할 수 있습니다. 응용 프로그램 시작 응용 프로그램에 대 한 검색 결과 항목에 사용자가을 누르면는 `TodoItemPage` 탐색 하 고 `TodoItem` 심층에서 참조 링크가 표시 됩니다.

SQLite 데이터베이스를 사용 하는 방법에 대 한 자세한 내용은 참조 [로컬 데이터베이스 작업을](~/xamarin-forms/app-fundamentals/databases.md)합니다.

> [!NOTE]
> Xamarin.Forms 응용 프로그램 인덱싱 및 기능을 연결 하는 전체 iOS 및 Android 플랫폼에서 제공 되 고 iOS 9 및 API 23 각각 필요 합니다.

## <a name="setup"></a>설정

다음 섹션에서는이 기능을 사용 하 여 iOS 및 Android 플랫폼에 대 한 추가 설치 지침을 제공 합니다.

### <a name="ios"></a>iOS

IOS 플랫폼에서이 기능을 사용 하는 데 필요한 추가 설정 없이 있습니다.

### <a name="android"></a>Android

Android 플랫폼에서 다양 한 응용 프로그램 인덱싱 및 딥 링크 기능을 사용 하려면 충족 해야 하는 필수 구성 요소입니다.

1. 응용 프로그램의 버전은 Google Play에서 사용 중인 이어야 합니다.
1. Google 개발자 콘솔에서 응용 프로그램에 대해 도우미 웹 사이트를 등록 해야 합니다. Url은 가능 응용 프로그램 웹 사이트와 연결 되 고 나면 웹 사이트 및 검색 결과에서 제공 될 수 있는 응용 프로그램 모두에 대해 작동 하는 인덱스입니다. 자세한 내용은 참조 [Google 검색에서 응용 프로그램 인덱싱을](https://support.google.com/googleplay/android-developer/answer/6041489) Google의 웹 사이트에 있습니다.
1. 응용 프로그램에서 HTTP URL 의도 지원 해야는 `MainActivity` 응용 프로그램이 응용 프로그램 인덱싱을 어떤 유형의 URL 스키마의 데이터를 알 수 있는 클래스에 응답할 수 있습니다. 자세한 내용은 참조 [의도 필터 구성](~/android/platform/app-linking.md#configure-intent-filter)합니다.

이러한 필수 구성이 요소가 충족 되 면 다음과 같은 추가 설치는 Xamarin.Forms 응용 프로그램 인덱싱 및 Android 플랫폼에 직접 링크를 사용 해야 합니다.

1. 설치는 [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android 응용 프로그램 프로젝트에 NuGet 패키지 합니다.
1. 에 `MainActivity.cs` 파일, 가져오기는 `Xamarin.Forms.Platform.Android.AppLinks` 네임 스페이스입니다.
1. 에 `MainActivity.OnCreate` 재정의 아래 코드의 다음 줄 추가 `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

자세한 내용은 참조 [딥 링크 사용 하 여 콘텐츠 Xamarin.Forms로 URL 탐색](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) Xamarin 블로그.

## <a name="indexing-a-page"></a>페이지 인덱싱

페이지 인덱싱 및 Google 및 스포트라이트 검색에 노출 하는 프로세스는 다음과 같습니다.

1. 만들기는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 사용자가 검색 결과에서 인덱싱된 내용이 선택 페이지로 돌아가서 딥 링크와 함께 페이지를 인덱싱하는 데 필요한 메타 데이터를 포함 합니다.
1. 등록 된 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 인스턴스 검색을 위해 인덱싱할 수 있습니다.

다음 코드 예제에서는 만드는 방법을 보여 줍니다.는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 인스턴스:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 인스턴스에 다양 한 값을 가진 인덱스의 페이지 하 고 딥 링크를 만드는 데 필요한 속성을 포함 합니다. [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), 및 [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) 속성 검색 결과에 표시 되 면 인덱싱된 콘텐츠를 식별 하는 데 사용 됩니다. [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) 속성이 `true` 인덱싱된 내용이 현재 보고 있는 나타냅니다. [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) 속성은 한 `Uri` 현재 페이지에 반환 하 여 현재 표시 하는 데 필요한 정보가 들어 있는 `TodoItem`합니다. 다음 예에서는 예를 보여 줍니다. `Uri` 샘플 응용 프로그램:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

이 `Uri` 시작 하는 데 필요한 모든 정보를 포함는 `deeplinking` 앱로 이동는 `DeepLinking.TodoItemPage`, 표시는 `TodoItem` 있는 `ID` 의 `ec38ebd1-811e-4809-8a55-0d028fce7819`합니다.

## <a name="registering-content-for-indexing"></a>콘텐츠 인덱싱에 등록 하는 중

한 번는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 인스턴스가 생성 된, 검색 결과에 표시를 인덱싱에 등록 해야 합니다. 이 사용 하 여 수행 되는 [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) 메서드를 다음 코드 예제에서와 같이:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

이렇게 하면 추가 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 응용 프로그램의 인스턴스 [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) 컬렉션입니다.

> [!NOTE]
> `RegisterLink` 메서드 페이지에 대 한 인덱싱된는 콘텐츠 업데이트를 사용할 수도 있습니다.

한 번는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 인스턴스에 등록 되어 인덱싱, 검색 결과에 나타날 수 있습니다. 다음 스크린샷은 iOS 플랫폼 검색 결과에 표시 되는 인덱싱된 콘텐츠:

![](deep-linking-images/ios-search.png "IOS에서 검색 결과에서 인덱싱된 콘텐츠")

## <a name="de-registering-indexed-content"></a>콘텐츠 인덱스 등록 취소

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) 다음 코드 예제에서와 같이 메서드 검색 결과에서 인덱싱된 콘텐츠 제거를 사용 합니다.

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

이렇게 하면 제거는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 에서 응용 프로그램의 인스턴스 [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) 컬렉션입니다.

> [!NOTE]
> Android에서은 검색 결과에서 인덱싱된 콘텐츠를 제거할 수 없습니다.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>딥 링크에 응답

인덱싱된 콘텐츠 검색 결과에 표시 되 고 사용자가 선택 되는 경우는 `App` 응용 프로그램 처리 하는 요청을 받게 됩니다에 대 한 클래스는 `Uri` 인덱싱된 내용이에 포함 합니다. 이 요청을 처리할 수는 [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) 다음 코드 예제에서와 같이 재정의:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) 메서드를 확인 하는 받은 `Uri` 구문 분석 하기 전에 응용 프로그램을 위한는 `Uri` 탐색 페이지 및 페이지에 전달할 매개 변수입니다. 가 만들어지면 탐색 페이지의 인스턴스 및 `TodoItem` 나타내는 매개 변수 페이지에서 검색 됩니다. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 이동 하 게 될 페이지의 다음 설정은 `TodoItem`합니다. 이렇게 하면 때는 `TodoItemPage` 하 여 표시 되는 [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) 메서드를 것 보여 줄는 `TodoItem` 인 `ID` 딥 링크에 포함 된 합니다.

## <a name="making-content-available-for-search-indexing"></a>검색 인덱싱에 대 한 콘텐츠 사용 가능

딥 링크를 나타내는 페이지가 표시 될 때마다는 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) 속성 설정할 수 있습니다 `true`합니다. 이렇게 하면 iOS 및 Android는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 검색 인덱싱 및 iOS에만 사용 가능한 인스턴스를 하기가 `AppLinkEntry` 전달 하기 위해 사용할 수 있는 인스턴스. 핸드 오프 하는 방법에 대 한 자세한 내용은 참조 [핸드 오프 소개](~/ios/platform/handoff.md)합니다.

다음 코드 예제에서는 설정 된 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) 속성을 `true` 에 [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) 재정의:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

마찬가지로, 딥 링크도 나타내는 페이지를 탐색는 경우에서 멀리는 [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) 속성 설정할 수 있습니다 `false`합니다. 이 인해 중지 iOS 및 Android는 [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) 보급 알림 되 고 검색 인덱싱 및 iOS에 대해서만 it에도 인스턴스 중지 광고는 `AppLinkEntry` 전달 하기 위해 인스턴스. 이 작업을 수행할 수 있습니다는 [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) 다음 코드 예제에서와 같이 재정의:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>핸드 오프를 데이터를 제공합니다.

Ios에서 응용 프로그램별 데이터 페이지를 인덱싱하는 경우 저장할 수 있습니다. 데이터를 추가 하 여 이렇게는 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) 컬렉션은 한 `Dictionary<string, string>` 전달에 사용 되는 키-값 쌍을 저장 하기 위한 합니다. 핸드 오프는 사용자가 자신의 장치 중 하나에 활동을 시작 하 고 (사용자의 iCloud 계정과으로 식별 됨) 처럼 다른 장치에서 해당 활동을 계속 하는 방법입니다. 다음 코드에서는 응용 프로그램별 키-값 쌍을 저장할 경우의 예를 보여 줍니다.

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

에 저장 된 값의 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) 컬렉션 인덱스 페이지에 대 한 메타 데이터에 저장 되 고 사용자가 딥 링크를 포함 하는 (또는 다른 콘텐츠를 보려면 핸드 오프 사용 되는 경우 검색 결과 누를 때 복원할 수 로그인 장치)입니다.

또한 다음 키에 대 한 값을 지정할 수 있습니다.

- `contentType` –는 `string` 인덱싱된 콘텐츠의 uniform 유형 식별자를 지정 하는입니다. 이 값에 대해 사용 하도록 권장 되는 규칙에는 인덱싱된 콘텐츠를 포함 하는 페이지의 형식 이름입니다.
- `associatedWebPage` –는 `string` 인덱싱된 콘텐츠는 웹에서 볼 수도 있습니다 또는 응용 프로그램 지원 Safari의 딥 링크를 방문 하 여 웹 페이지를 나타내는입니다.
- `shouldAddToPublicIndex` –는 `string` 의 `true` 또는 `false` 인덱싱된 콘텐츠 다음 iOS 장치에 응용 프로그램을 설치 하지 않은 사용자에 게 제공할 수 있는 Apple의 공용 클라우드 인덱스를 추가할 것인지 여부를 제어 하 합니다. 그러나 해 서 콘텐츠 공용 인덱싱에 대해 설정 되어 있는지 자동으로 추가 됩니다 Apple의 공용 클라우드 인덱스로 것은 아닙니다. 자세한 내용은 참조 [공용 검색 인덱싱](~/ios/platform/search/nsuseractivity.md)합니다. 이 키로 설정 해야 하는 참고 `false` 개인 데이터를 추가 하는 경우는 [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) 컬렉션입니다.

> [!NOTE]
> `KeyValues` 컬렉션 Android 플랫폼에서 사용 되지 않습니다.

핸드 오프 하는 방법에 대 한 자세한 내용은 참조 [핸드 오프 소개](~/ios/platform/handoff.md)합니다.

## <a name="summary"></a>요약

이 문서에는 응용 프로그램 인덱싱을 사용 하는 방법과 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 하는 직접 링크 명시 합니다. 응용 프로그램 인덱싱을 응용 프로그램을 그렇지 않은 경우 무시 될에 대 한 몇 가지 사용 후 검색 결과에 표시 하 여 관련 정보를 얻을 수 있습니다.


## <a name="related-links"></a>관련 링크

- [딥 링크 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS 검색 Api](~/ios/platform/search/index.md)
- [Android 6.0에서 응용 프로그램 연결](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
