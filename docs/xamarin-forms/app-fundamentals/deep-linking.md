---
title: 애플리케이션 인덱싱 및 딥 링크 설정
description: 이 문서에서는 애플리케이션 인덱싱 및 딥 링크를 사용하여 iOS 및 Android 디바이스에서 Xamarin.Forms 애플리케이션 콘텐츠를 검색 가능하게 하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/28/2018
ms.openlocfilehash: e7b8ae57f127b4c9397ab4e5f7e097fa330e827a
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67650654"
---
# <a name="application-indexing-and-deep-linking"></a>애플리케이션 인덱싱 및 딥 링크 설정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/DeepLinking/)

_애플리케이션 인덱싱은 검색 결과에 나타나서 관련성을 유지하기 위해 몇 번 사용한 후에 잊혀질 애플리케이션을 허용합니다. 딥 링크 설정은 애플리케이션이 애플리케이션 데이터를 포함하는 검색 결과에 응답할 수 있도록 하며, 일반적으로 딥 링크에서 참조하는 페이지로 이동하면 됩니다. 이 문서에서는 애플리케이션 인덱싱 및 딥 링크를 사용하여 iOS 및 Android 디바이스에서 Xamarin.Forms 애플리케이션 콘텐츠를 검색 가능하게 하는 방법을 설명합니다._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Xamarin.Forms 및 Azure와 딥 링크 설정 동영상**


Xamarin.Forms 애플리케이션 인덱싱 및 딥 링크 설정은 사용자가 애플리케이션을 탐색할 때 애플리케이션 인덱싱에 대한 메타데이터를 게시하기 위한 API를 제공합니다. 그런 다음, 스포트라이트 검색, Google 검색 또는 웹 검색에서 인덱싱된 콘텐츠를 검색할 수 있습니다. 딥 링크가 포함된 검색 결과를 누르면 애플리케이션에서 처리할 수 있는 이벤트가 발생하고 일반적으로 딥 링크에서 참조되는 페이지로 이동하는 데 사용됩니다.

이 샘플 애플리케이션은 다음 스크린샷과 같이 데이터가 로컬 SQLite 데이터베이스에 저장되는 Todo 목록 애플리케이션을 보여줍니다.

![](deep-linking-images/screenshots.png "TodoList 애플리케이션")

사용자가 만든 각 `TodoItem` 인스턴스는 인덱싱됩니다. 그런 다음, 플랫폼별 검색을 사용하여 애플리케이션에서 인덱싱된 데이터를 찾을 수 있습니다. 사용자 애플리케이션에 대한 검색 결과 항목을 누르면 애플리케이션이 시작되고 `TodoItemPage`가 탐색되고, 딥 링크에서 참조되는 `TodoItem`이 표시됩니다.

SQLite 데이터베이스 사용에 대한 자세한 내용은 [Xamarin.Forms 로컬 데이터베이스](~/xamarin-forms/data-cloud/data/databases.md)를 참조하세요.

> [!NOTE]
> Xamarin.Forms 애플리케이션 인덱싱 및 딥 링크 기능은 iOS 및 Android 플랫폼에서만 사용할 수 있으며 각각 최소 iOS 9 및 API 23이 필요합니다.

## <a name="setup"></a>설정

다음 섹션에서는 iOS 및 Android 플랫폼에서 이 기능을 사용하기 위한 추가 설치 지침을 제공합니다.

### <a name="ios"></a>iOS

iOS 플랫폼에서 iOS 플랫폼 프로젝트가 **Entitlements.plist** 파일을 번들 서명에 대한 사용자 지정 자격 파일로 설정했는지 확인합니다.

iOS 유니버설 링크를 사용하려면:

1. 앱에서 지원할 모든 도메인을 포함하여 `applinks` 키와 함께 앱에 연결된 도메인 자격을 추가합니다.
1. 웹 사이트에 Apple 앱 사이트 연결 파일을 추가합니다.
1. `applinks` 키를 Apple 앱 사이트 연결 파일에 추가합니다.

자세한 내용은 developer.apple.com에서 [앱 및 웹 사이트가 콘텐츠에 링크되도록 허용](https://developer.apple.com/documentation/uikit/core_app/allowing_apps_and_websites_to_link_to_your_content)을 참조하세요.

### <a name="android"></a>Android

Android 플랫폼에서 애플리케이션 인덱싱 및 딥 링크 기능을 사용하기 위해 충족해야 하는 몇 가지 필수 구성 요소가 있습니다.

1. 애플리케이션은 Google Play에 게시되어야 합니다.
1. 동반 웹 사이트는 Google 개발자 콘솔의 애플리케이션에 등록되어야 합니다. 애플리케이션이 웹 사이트와 연결되면 웹 사이트와 애플리케이션 모두에서 작동하는 URL을 인덱싱할 수 있습니다. 이는 검색 결과에 제공될 수 있습니다. 자세한 내용은 Google 웹 사이트의 [Google의 검색에서 앱 인덱싱](https://support.google.com/googleplay/android-developer/answer/6041489)을 참조하세요.
1. 애플리케이션이 응답할 수 있는 URL 데이터 구성 스키마의 유형을 알려주는 `MainActivity` 클래스의 HTTP URL 의도를 애플리케이션이 지원해야 합니다. 자세한 내용은 [의도 필터 구성](~/android/platform/app-linking.md#configure-intent-filter)을 참조하세요.

이러한 필수 구성 요소가 충족되면 Android 플랫폼에서 Xamarin.Forms 애플리케이션 인덱싱 및 딥 링크를 사용하기 위해 다음 추가 설치가 필요합니다.

1. [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) NuGet 패키지를 Android 애플리케이션 프로젝트에 설치합니다.
1. **MainActivity.cs** 파일에 `Xamarin.Forms.Platform.Android.AppLinks` 네임스페이스를 사용하여 선언을 추가합니다.
1. **MainActivity.cs** 파일에 `Firebase` 네임스페이스를 사용하여 선언을 추가합니다.
1. 웹 브라우저에서 [Firebase 콘솔](https://console.firebase.google.com/)을 통해 새 프로젝트를 만듭니다.
1. Firebase 콘솔에서 Firebase를 Android 앱에 추가하고 필요한 데이터를 입력합니다.
1. 결과 **google-services.json** 파일을 다운로드합니다.
1. **google-services.json** 파일을 Android 프로젝트의 루트 디렉터리에 추가하고, 해당 **빌드 작업**을 **GoogleServicesJson**으로 설정합니다.
1. `MainActivity.OnCreate` 재정의에서 `Forms.Init(this, bundle)` 아래에 다음 코드 줄을 추가합니다.

```csharp
FirebaseApp.InitializeApp(this);
AndroidAppLinks.Init(this);
```

**google-services.json**이 프로젝트에 추가되고 (및 *GoogleServicesJson** 빌드 작업이 설정) 빌드 프로세스에서 클라이언트 ID 및 API 키를 추출한 다음, 생성된 매니페스트 파일에 이러한 자격 증명을 추가합니다.

자세한 내용은 Xamarin 블로그에서 [Xamarin.Forms URL 탐색이 있는 딥 링크 콘텐츠](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)를 참조하세요.

## <a name="indexing-a-page"></a>페이지 인덱싱

페이지를 인덱싱하고 Google 및 스포트라이트에 검색에 노출하는 프로세스는 다음과 같습니다.

1. 사용자가 검색 결과에서 인덱싱된 콘텐츠를 선택할 때 페이지에 반환하는 딥 링크와 함께 페이지를 인덱싱하는 데 필요한 메타데이터가 포함된 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry)를 만듭니다.
1. 검색을 위해 인덱싱하려면 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스를 등록합니다.

다음 코드 예제는 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스를 만드는 방법을 보여줍니다.

```csharp
AppLinkEntry GetAppLink(TodoItem item)
{
    var pageType = GetType().ToString();
    var pageLink = new AppLinkEntry
    {
        Title = item.Name,
        Description = item.Notes,
        AppLinkUri = new Uri($"http://{App.AppName}/{pageType}?id={item.ID}", UriKind.RelativeOrAbsolute),
        IsLinkActive = true,
        Thumbnail = ImageSource.FromFile("monkey.png")
    };

    pageLink.KeyValues.Add("contentType", "TodoItemPage");
    pageLink.KeyValues.Add("appName", App.AppName);
    pageLink.KeyValues.Add("companyName", "Xamarin");

    return pageLink;
}
```

[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스에는 페이지를 인덱싱하고 딥 링크를 만드는 데 필요한 값이 있는 여러 속성이 포함되어 있습니다. [`Title`](xref:Xamarin.Forms.IAppLinkEntry.Title), [`Description`](xref:Xamarin.Forms.IAppLinkEntry.Description) 및 [`Thumbnail`](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) 속성이 검색 결과에 표시될 때 인덱싱된 콘텐츠를 식별하는 데 사용됩니다. [`IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성이 `true`로 설정되어 인덱싱된 콘텐츠가 현재 보고 있음을 나타냅니다. [`AppLinkUri`](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) 속성은 현재 페이지로 반환하여 현재 `TodoItem`을 표시하는 데 필요한 정보를 포함하는 `Uri`입니다. 다음 예제에서는 샘플 애플리케이션에 대한 예제 `Uri`를 보여줍니다.

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=2
```

이 `Uri`에는 `deeplinking` 앱을 시작하고, `DeepLinking.TodoItemPage`로 이동하여 `ID`가 2인 `TodoItem`을 표시하는 데 필요한 모든 정보가 들어 있습니다.

## <a name="registering-content-for-indexing"></a>인덱싱할 콘텐츠 등록

[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 생성되면 인덱싱이 검색 결과에 나타나도록 등록해야 합니다. 이 작업은 다음 코드 예제와 같이 [`RegisterLink`](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) 메서드를 사용하여 수행할 수 있습니다.

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

이렇게 하면 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 애플리케이션의 [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) 컬렉션에 추가됩니다.

> [!NOTE]
> `RegisterLink` 메서드를 사용하여 페이지에 대해 인덱싱된 콘텐츠를 업데이트할 수도 있습니다.

[`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 인덱싱을 위해 등록되면 검색 결과에 나타날 수 있습니다. 다음 스크린샷은 iOS 플랫폼의 검색 결과에 나타나는 인덱싱된 콘텐츠를 보여줍니다.

![](deep-linking-images/ios-search.png "iOS의 검색 결과에서 인덱싱된 콘텐츠")

## <a name="de-registering-indexed-content"></a>인덱싱된 콘텐츠 등록 취소

[`DeregisterLink`](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) 메서드는 다음 코드 예제에 설명된 대로 검색 결과에서 인덱싱된 콘텐츠를 제거하는 데 사용됩니다.

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

이렇게 하면 애플리케이션의 [`AppLinks`](xref:Xamarin.Forms.Application.AppLinks) 컬렉션에서 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 제거됩니다.

> [!NOTE]
> Android에서는 검색 결과에서 인덱싱된 콘텐츠를 제거할 수 없습니다.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>딥 링크에 응답

인덱싱된 콘텐츠가 검색 결과에 나타나고 사용자가 선택한 경우 애플리케이션의 `App` 클래스는 인덱싱된 콘텐츠에 포함된 `Uri`를 처리하라는 요청을 받습니다. 이 요청은 다음 코드 예제에 설명된 대로 [`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) 재정의에서 처리할 수 있습니다.

```csharp
public class App : Application
{
    ...
    protected override async void OnAppLinkRequestReceived(Uri uri)
    {
        string appDomain = "http://" + App.AppName.ToLowerInvariant() + "/";
        if (!uri.ToString().ToLowerInvariant().StartsWith(appDomain, StringComparison.Ordinal))
            return;

        string pageUrl = uri.ToString().Replace(appDomain, string.Empty).Trim();
        var parts = pageUrl.Split('?');
        string page = parts[0];
        string pageParameter = parts[1].Replace("id=", string.Empty);

        var formsPage = Activator.CreateInstance(Type.GetType(page));
        var todoItemPage = formsPage as TodoItemPage;
        if (todoItemPage != null)
        {
            var todoItem = await App.Database.GetItemAsync(int.Parse(pageParameter));
            todoItemPage.BindingContext = todoItem;
            await MainPage.Navigation.PushAsync(formsPage as Page);
        }
        base.OnAppLinkRequestReceived(uri);
    }
}
```

[`OnAppLinkRequestReceived`](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) 메서드는 `Uri`를 탐색할 페이지로 구문 분석하고 매개 변수를 페이지에 전달하기 전에 수신된 `Uri`가 애플리케이션용인지를 확인합니다. 탐색할 페이지의 인스턴스가 생성되고 페이지 매개 변수로 표시되는 `TodoItem`이 검색됩니다. 탐색할 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)가 `TodoItem`으로 설정됩니다. 이렇게 하면 `TodoItemPage`가 [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) 메서드로 표시될 때 딥 링크에 `ID`가 포함된 `TodoItem`가 표시됩니다.

## <a name="making-content-available-for-search-indexing"></a>검색 인덱싱에 사용할 수 있는 콘텐츠 만들기

딥 링크로 나타나는 페이지가 표시될 때마다 [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성을 `true`로 설정할 수 있습니다. 이로 인해 iOS 및 Android에서는 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 검색 인덱싱에 사용 가능하며 iOS에서만 `AppLinkEntry` 인스턴스를 핸드오프에 사용할 수 있게 됩니다. 핸드오프에 대한 자세한 내용은 [핸드오프 소개](~/ios/platform/handoff.md)를 참조하세요.

다음 코드 예제는 [`Page.OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 재정의에서 [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성을 `true`로 설정하는 방법을 보여줍니다.

```csharp
protected override void OnAppearing()
{
    appLink = GetAppLink(BindingContext as TodoItem);
    if (appLink != null)
    {
        appLink.IsLinkActive = true;
    }
}
```

마찬가지로 딥 링크로 표시된 페이지를 탐색할 때 [`AppLinkEntry.IsLinkActive`](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) 속성을 `false`로 설정할 수 있습니다. 이렇게 하면 iOS 및 Android에서 검색 인덱싱용으로 알려진 [`AppLinkEntry`](xref:Xamarin.Forms.AppLinkEntry) 인스턴스가 중지되고 iOS에서만 핸드오프용으로 알려진 `AppLinkEntry` 인스턴스도 중지됩니다. 이 작업은 다음 코드 예제에 설명된 대로 [`Page.OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 재정의에서 수행할 수 있습니다.

```csharp
protected override void OnDisappearing()
{
    if (appLink != null)
    {
        appLink.IsLinkActive = false;
    }
}
```

## <a name="providing-data-to-handoff"></a>핸드오프에 데이터 제공

iOS에서는 페이지를 인덱싱할 때 애플리케이션별 데이터를 저장할 수 있습니다. 이는 핸드오프에 사용되는 키-값 쌍을 저장하기 위한 `Dictionary<string, string>`인 [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 컬렉션에 데이터를 추가하여 수행됩니다. 핸드오프는 사용자가 해당 디바이스 중 하나에서 작업을 시작하고 해당 디바이스의 다른 작업(사용자의 iCloud 계정으로 식별됨)을 계속하기 위한 방법입니다. 다음 코드는 애플리케이션별 키-값 쌍을 저장하는 예를 보여줍니다.

```csharp
var pageLink = new AppLinkEntry
{
    ...
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

[`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 컬렉션에 저장된 값은 인덱싱된 페이지의 메타데이터에 저장되고, 사용자가 딥 링크를 포함하는 검색 결과(또는 다른 로그인 디바이스의 콘텐츠를 보는 데 핸드오프를 사용하는 경우)를 누르면 복원됩니다.

또한 다음 키에 대한 값을 지정할 수 있습니다.

- `contentType` – 인덱싱된 콘텐츠의 균일한 유형 식별자를 지정하는 `string`입니다. 이 값에 대해 사용하도록 권장되는 규칙은 인덱싱된 콘텐츠를 포함하는 페이지의 형식 이름입니다.
- `associatedWebPage` – 인덱싱된 콘텐츠도 웹에서 볼 수 있거나 애플리케이션이 Safari의 딥 링크를 지원하는 경우 방문할 웹 페이지를 나타내는 `string`입니다.
- `shouldAddToPublicIndex` - `true` 또는 `false` 중 하나인 `string`은 인덱싱된 콘텐츠를 Apple의 공용 클라우드 인덱스에 추가할지 여부를 제어하며, 해당 iOS 디바이스에 애플리케이션을 설치하지 않은 사용자에게 제공할 수 있습니다. 그러나 콘텐츠가 공개 인덱스에 대해 설정되어 있어도 Apple의 공용 클라우드 인덱스에 자동으로 추가되지는 않습니다. 자세한 내용은 [공용 검색 인덱싱](~/ios/platform/search/nsuseractivity.md)을 참조하세요. 개인 데이터를 [`KeyValues`](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) 컬렉션에 추가할 때 이 키를 `false`로 설정해야 합니다.

> [!NOTE]
> `KeyValues` 컬렉션은 Android 플랫폼에서 사용되지 않습니다.

핸드오프에 대한 자세한 내용은 [핸드오프 소개](~/ios/platform/handoff.md)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 애플리케이션 인덱싱 및 딥 링크를 사용하여 iOS 및 Android 디바이스에서 Xamarin.Forms 애플리케이션 콘텐츠를 검색 가능하게 하는 방법을 설명합니다. 애플리케이션 인덱싱을 사용하면 몇 번 사용한 후에 잊혀질 수 있는 검색 결과에 나타나서 애플리케이션의 관련성을 유지할 수 있게 합니다.

## <a name="related-links"></a>관련 링크

- [딥 링크(샘플)](https://developer.xamarin.com/samples/xamarin-forms/DeepLinking/)
- [iOS 검색 API](~/ios/platform/search/index.md)
- [Android 6.0에 앱 연결](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
