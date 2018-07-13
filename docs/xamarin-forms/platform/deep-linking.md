---
title: 응용 프로그램 인덱싱 및 딥 링크 설정
description: 이 문서는 응용 프로그램 인덱싱을 사용 하는 방법 및 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 딥 링크 설정 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996098"
---
# <a name="application-indexing-and-deep-linking"></a>응용 프로그램 인덱싱 및 딥 링크 설정

_응용 프로그램 인덱싱 몇 가지 사용 하 여 검색 결과에 표시 하 여 인재를 후 잊어버린 그렇지 않은 경우는 응용 프로그램을 수 있습니다. 딥 링크 설정 응용 프로그램을 딥 링크에서 참조 하는 페이지로 이동 하 여 일반적으로 응용 프로그램 데이터를 포함 하는 검색 결과에 응답할 수 있습니다. 이 문서는 응용 프로그램 인덱싱을 사용 하는 방법 및 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 딥 링크 설정 방법을 보여 줍니다._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**깊이 Xamarin.Forms와 Azure를 사용 하 여 링크 [Xamarin University](https://university.xamarin.com/)**


Xamarin.Forms 응용 프로그램 인덱싱 및 딥 링크 설정 응용 프로그램 사용자가 응용 프로그램을 탐색 하는 대로 인덱싱에 대 한 메타 데이터 게시에 대 한 API를 제공 합니다. 스포트라이트 검색, Google 검색 또는 웹 검색에서에 대 한 인덱싱된 콘텐츠를 검색한 다음 있습니다. 딥 링크를 포함 하는 검색 결과 탭에는 응용 프로그램에서 처리할 수는 딥 링크에서 참조 하는 페이지를 탐색 하는 데 일반적으로 이벤트를 발생 합니다.

샘플 응용 프로그램은 다음 스크린샷과에서 같이 로컬 SQLite 데이터베이스에서 데이터를 저장 된 Todo 목록 응용 프로그램을 보여 줍니다.

![](deep-linking-images/screenshots.png "TodoList 응용 프로그램")

각 `TodoItem` 인덱싱된 사용자가 만든 인스턴스입니다. 다음 플랫폼별 검색 응용 프로그램에서 인덱싱된 데이터를 찾는 데 사용할 수 있습니다. 사용자 응용 프로그램에 대 한 검색 결과 항목에 누르면 응용 프로그램이 시작 되는 `TodoItemPage` 탐색 및 `TodoItem` 심층에서 참조 링크가 표시 됩니다.

SQLite 데이터베이스를 사용 하는 방법에 대 한 자세한 내용은 참조 하세요. [로컬 데이터베이스를 사용 하 여 작업](~/xamarin-forms/app-fundamentals/databases.md)합니다.

> [!NOTE]
> Xamarin.Forms 응용 프로그램 인덱싱 및 딥 링크 기능 에서만 사용 가능 iOS 및 Android 플랫폼에서 하며 iOS 9 및 API 23 각각.

## <a name="setup"></a>설정

다음 섹션에서는 iOS 및 Android 플랫폼에서이 기능을 사용 하는 것에 대 한 추가 설치 지침을 제공 합니다.

### <a name="ios"></a>iOS

IOS 플랫폼에서이 기능을 사용 하는 데 필요한 추가 설치 없이 있습니다.

### <a name="android"></a>Android

Android 플랫폼에서 다양 한 응용 프로그램 인덱싱 및 딥 링크 기능을 사용 하려면 충족 해야 하는 필수 구성 요소입니다.

1. Google Play에서 응용 프로그램의 버전을 사용 해야 합니다.
1. 동반 웹 사이트를 Google 개발자 콘솔에서 응용 프로그램에 등록 되어야 합니다. Url 수 응용 프로그램을 웹 사이트와 연결 되 면 웹 사이트 및 검색 결과에 제공 될 수 있는 응용 프로그램 모두에 대해 작동 하는 인덱스입니다. 자세한 내용은 [앱이 Google 검색 인덱싱](https://support.google.com/googleplay/android-developer/answer/6041489) Google의 웹 사이트입니다.
1. 응용 프로그램에서 HTTP URL 의도 지원 해야 합니다는 `MainActivity` 응용 프로그램 URL 데이터 스키마의 유형을 인덱싱 응용 프로그램에 지시 하는 클래스에 응답할 수 있습니다. 자세한 내용은 [의도 필터 구성](~/android/platform/app-linking.md#configure-intent-filter)합니다.

이러한 필수 구성이 요소가 충족 되 면 다음 추가 설치는 Xamarin.Forms 응용 프로그램 인덱싱 및 Android 플랫폼에서 딥 링크를 사용 해야 합니다.

1. 설치 합니다 [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android 응용 프로그램 프로젝트에 NuGet 패키지.
1. 에 `MainActivity.cs` 파일에서 가져오기는 `Xamarin.Forms.Platform.Android.AppLinks` 네임 스페이스입니다.
1. 에 `MainActivity.OnCreate` 재정의 아래 코드의 다음 줄을 추가 `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

자세한 내용은 [딥 링크 콘텐츠 Xamarin.Forms URL 탐색이 있는](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) Xamarin 블로그.

## <a name="indexing-a-page"></a>페이지를 인덱싱

페이지를 인덱싱 및 검색 Google 및 추천에 노출에 대 한 프로세스는 다음과 같습니다.

1. 만들기는 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 사용자가 검색 결과에서 인덱싱된 콘텐츠를 선택 하면 페이지에 반환 하는 딥 링크와 함께 페이지를 인덱스에 필요한 메타 데이터를 포함 하는 합니다.
1. 등록 된 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 인스턴스를 검색 하는 것에 대 한 인덱스입니다.

다음 코드 예제에서는 만드는 방법을 보여 줍니다.는 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 인스턴스:

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

합니다 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 인스턴스에 다양 한 속성 페이지를 인덱싱 및 딥 링크 만들기 하는 데 필요한 값을 포함 합니다. 합니다 [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), 및 [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) 속성 검색 결과에서 표시 되는 인덱싱된 콘텐츠 식별을 사용 합니다. [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성 `true` 인덱싱된 내용이 현재 보고 있는 나타냅니다. 합니다 [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) 속성이 `Uri` 현재 페이지에 반환 하 여 현재 표시 하는 데 필요한 정보가 들어 있는 `TodoItem`합니다. 다음 예에서는 예를 보여 줍니다. `Uri` 샘플 응용 프로그램:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

이 `Uri` 시작 하는 데 필요한 모든 정보를 포함 합니다 `deeplinking` 앱 이동할를 `DeepLinking.TodoItemPage`, 표시 및를 `TodoItem` 있는 `ID` 의 `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>콘텐츠 인덱싱을 위해 등록

한 번을 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 인스턴스를 만들고, 검색 결과에 표시 되도록 인덱싱을 위해 등록 해야 합니다. 사용 하 여 이렇게 합니다 [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) 메서드를 다음 코드 예제에서 설명한 것 처럼:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

이 추가 합니다 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 응용 프로그램의 인스턴스 [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) 컬렉션입니다.

> [!NOTE]
> `RegisterLink` 메서드 페이지에 대 한 인덱싱된는 콘텐츠 업데이트를 사용할 수도 있습니다.

한 번을 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 인스턴스에 등록 된 인덱싱에 대 한 검색 결과에 나타날 수 있습니다. 다음 스크린샷은 iOS 플랫폼에서 검색 결과에 표시 되는 인덱싱된 콘텐츠:

![](deep-linking-images/ios-search.png "IOS에서 검색 결과에서 인덱싱된 콘텐츠")

## <a name="de-registering-indexed-content"></a>콘텐츠를 인덱싱할 등록 취소

합니다 [ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) 다음 코드 예제에서 설명한 것 처럼 메서드 검색 결과에서 인덱싱된 콘텐츠 제거를 사용 합니다.

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

이 제거 합니다 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) 응용 프로그램의 인스턴스 [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) 컬렉션입니다.

> [!NOTE]
> Android에서 검색 결과에서 인덱싱된 콘텐츠를 제거할 수 없는 합니다.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>딥 링크에 응답

인덱싱된 콘텐츠가 검색 결과에 표시 되 고 사용자가 선택 되어는 `App` 응용 프로그램 처리에 요청을 받게 됩니다에 대 한 클래스를 `Uri` 인덱싱된 내용이에 포함 합니다. 이 요청에서 처리할 수는 [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) 재정의 다음 코드 예제에서 설명한 것 처럼:

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

[ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) 메서드를 확인 하는 받은 데이터를 `Uri` 구문 분석 하기 전에 응용 프로그램을 위한 것은 `Uri` 이동 하 게 될 페이지 및 페이지에 전달할 매개 변수를 합니다. 만들어진 탐색 페이지의 인스턴스 및 `TodoItem` 표시 페이지 매개 변수를 검색 합니다. 합니다 [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 이동 하 게 될 페이지의 설정 됩니다는 `TodoItem`합니다. 때 이렇게를 `TodoItemPage` 에 표시 되는 [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) 메서드를이 표시 됩니다는 `TodoItem` 인 `ID` 딥 링크에 포함 된.

## <a name="making-content-available-for-search-indexing"></a>콘텐츠를 검색 인덱싱에 사용할 수 있도록 설정

딥 링크에서 나타내는 페이지 표시 될 때마다 합니다 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성 설정할 수 있습니다 `true`합니다. 이렇게 하면 iOS 및 Android에서 합니다 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) iOS에만 해당 및 검색 인덱싱에 대해 사용 가능한 경우 하기가 `AppLinkEntry` 핸드 오프에 대 한 사용 가능한 인스턴스. 핸드 오프에 대 한 자세한 내용은 참조 하세요. [핸드 오프 소개](~/ios/platform/handoff.md)합니다.

다음 코드 예제에서는 설정 합니다 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성을 `true` 에 [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 재정의:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

마찬가지로, 딥 링크에서 나타내는 페이지를 탐색는 경우에서 멀리 합니다 [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성 설정할 수 있습니다 `false`합니다. IOS 및 Android에서이 중지 합니다 [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) iOS 에서만 it 검색 인덱싱에 대 한 한도 알리려는 인스턴스 중지 광고를 `AppLinkEntry` 핸드 오프에 대 한 인스턴스. 이 작업을 수행할 수 있습니다 합니다 [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 재정의 다음 코드 예제에서 설명한 것 처럼:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>핸드 오프에 데이터를 제공합니다.

Ios의 경우 응용 프로그램별 데이터 페이지를 인덱싱할 때 저장할 수 있습니다. 데이터를 추가 하 여 이렇게 합니다 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 인 컬렉션을 `Dictionary<string, string>` 전달에 사용 되는 키-값 쌍을 저장 하기 위한 합니다. 핸드 오프 방법은 사용자가 해당 장치 중 하나에서 작업을 시작 하 고 (사용자의 iCloud 계정으로 식별 됨) 하는 대로 해당 장치의 다른 작업을 계속 합니다. 다음 코드는 응용 프로그램별 키-값 쌍을 저장 하는 예를 보여 줍니다.

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

에 저장 된 값을 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 컬렉션 인덱스 페이지에 대 한 메타 데이터에 저장 되 고 사용자가 딥 링크를 포함 하는 (또는 다른 콘텐츠를 보려면 핸드 오프가 사용 하는 경우 검색 결과 누를 때 복원 됩니다 로그인 장치)입니다.

또한 다음 키에 대 한 값을 지정할 수 있습니다.

- `contentType` –는 `string` 인덱싱된 내용이의 균일 한 유형 식별자를 지정 하는 합니다. 이 값에 대해 사용 하도록 권장 되는 규칙에는 인덱싱된 콘텐츠를 포함 하는 페이지 형식 이름입니다.
- `associatedWebPage` –는 `string` 인덱싱된 콘텐츠는 웹에서 볼 수도 있습니다 또는 응용 프로그램 지원 Safari의 딥 링크를 방문 하 여 웹 페이지를 나타내는입니다.
- `shouldAddToPublicIndex` -는 `string` 중 `true` 또는 `false` 인덱싱된 콘텐츠는 iOS 장치에서 응용 프로그램을 설치 하지 않은 사용자에 게 제공할 수 있습니다는 Apple의 공용 클라우드 인덱스에 추가할 것인지 여부를 제어 하는 합니다. 그러나 공용 인덱싱에 대 한 콘텐츠를 설정한 단지는 자동으로 추가 됩니다 Apple의 공용 클라우드 인덱스로 것은 아닙니다. 자세한 내용은 [공용 검색 인덱싱](~/ios/platform/search/nsuseractivity.md)합니다. 이 키를 설정 해야 하는 참고 `false` 개인 데이터를 추가 하는 경우는 [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 컬렉션입니다.

> [!NOTE]
> `KeyValues` 컬렉션 Android 플랫폼에서 사용 되지 않습니다.

핸드 오프에 대 한 자세한 내용은 참조 하세요. [핸드 오프 소개](~/ios/platform/handoff.md)합니다.

## <a name="summary"></a>요약

이 문서에서는 응용 프로그램 인덱싱을 사용 하는 방법 및 Xamarin.Forms 응용 프로그램 콘텐츠를 iOS 및 Android 장치에서 검색할 수 있도록 딥 링크를 보여 줍니다. 응용 프로그램 인덱싱 통해 몇 가지 사용 후에 대 한 기억 그렇지 않은 경우는 검색 결과에 표시 하 여 관련 정보를 얻을 수 있습니다.


## <a name="related-links"></a>관련 링크

- [딥 링크 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS 검색 Api](~/ios/platform/search/index.md)
- [Android 6.0에 앱 연결](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
