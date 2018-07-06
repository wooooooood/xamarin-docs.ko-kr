---
title: Xamarin.Forms 플러그 인을 만들고 사용
description: 이 문서에서는 Xamarin.Forms 플러그 인을 만들고 사용 하는 방법을 설명 합니다. 플러그 인은 쉽게 네이티브 플랫폼 기능을 노출 하려면 일반적으로 사용 됩니다.
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
ms.openlocfilehash: 4d121c2dfcca380e1735da1a4ca47c42d1957b8a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854742"
---
# <a name="consuming-and-creating-xamarinforms-plugins"></a>Xamarin.Forms 플러그 인을 만들고 사용

모든 플랫폼에 걸쳐 있는 많은 네이티브 플랫폼 기능 하지만 약간 다른 Api가 있습니다. 이러한 기능을 사용 하는 개발자를 위한 한 가지 방법은 추상 플랫폼 간 인터페이스를 만들고 다음 다양 한 플랫폼에서 해당 인터페이스를 구현입니다. 그러면 Xamarin.Forms 응용 프로그램 액세스를 사용 하 여 이러한 플랫폼 구현을 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)합니다.

개발자가 작성 하 여이 작업을 공유할 수는 _플러그 인_ 및 NuGet에 게시 합니다.

> [!NOTE]
> 이전에 플러그 인을 통해서만 사용할 수 있는 많은 플랫폼 간 기능에 속함 이제 오픈 소스 **[Xamarin.Essentials](~/essentials/index.md)** 라이브러리입니다. 이러한 기능에 포함 됩니다: 배터리 상태, 나침반, 동작 센서, 지리적 위치, 텍스트 음성 변환, 및 훨씬 더 합니다. 향후 **Xamarin.Essentials** Xamarin.Forms 응용 프로그램에 대 한 플랫폼 간 기능의 주된 원인이 됩니다. 여전히 개발자가 만들고 플러그 인을 게시할 수 있습니다, 있지만에 영향을 주는 고려 **Xamarin.Essentials**합니다.

## <a name="finding-and-adding-plugins"></a>찾기 및 플러그 인 추가

Xamarin 커뮤니티는 Xamarin.Forms를 사용 하 여 호환 가능한 대부분의 플랫폼 간 플러그 인을 만들었습니다. 큰 컬렉션에서 찾을 수 있습니다.

[**Xamarin 플러그 인**](https://github.com/xamarin/XamarinComponents)

프로젝트에 NuGet 패키지를 추가 하는 가이드의 연습을 참조 하세요 [프로젝트에서 NuGet 패키지를 포함 하 여](/visualstudio/mac/nuget-walkthrough/)입니다.

## <a name="creating-plugins"></a>플러그 인 만들기

도 Nuget 패키지 (및 Xamarin 구성 요소)로 사용자 고유의 플러그 인을 게시 하는 것이 가능 합니다. 대부분의 기존 플러그 인 오픈 소스 되므로 writtern 된 방식을 이해 하려면 해당 코드를 검토할 수 있습니다.

예를 들어, 아래 플러그 인의 목록 모든 오픈 소스 이며에서 몇 가지 샘플에 해당 하는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 섹션:

- **Text to speech** James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) 고 [NuGet  ](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **배터리 상태** James montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) 고 [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

에 대 한 이러한 지침을 수행 하는 대로 해당 Github 프로젝트 고유한 플랫폼 간 플러그 인을 만들기 위한 좋은 시작 지점이 제공할 수 있습니다 [Xamarin에 대 한 플러그 인 만들기](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin)합니다.

### <a name="structuring-cross-platform-plugin-projects"></a>플랫폼 간 플러그 인 프로젝트를 구성합니다.

NuGet 패키지를 디자인 하기 위한 특정 요구 사항이 없습니다 있지만, 플랫폼 간 앱에 대 한 패키지를 만들기 위한 지침이 있습니다.

이전에 플랫폼 간 플러그 인을 일반적으로 다음 구성 요소 구성:

- 이 플러그 인에 대 한 API를 나타내는 인터페이스를 사용 하 여 PCL
- iOS, Android 및 유니버설 Windows 플랫폼 (UWP) 클래스 인터페이스의 구현 사용 하 여 라이브러리입니다.

읽기 James Montemagno [블로그 게시물](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) Xamarin에 대 한 플러그 인을 만드는 과정을 설명 합니다.

최근에 플러그 인 만들 수 있습니다 수 단일 멀티 타기 팅 플랫폼을 사용 하 여. 이 방법은 James Montemagno의 부분은 [블로그 게시물](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/)합니다. 이 방법은 위에 링크 되어 James Montemagno의 플러그 인에서 사용 되 고 형식에도 사용 됩니다 **Xamarin.Essentials**합니다.

것 Xamarin.Forms 플러그 인에서 직접 참조 하지 않으려면이 좋습니다.
다른 개발자가 플러그 인을 사용 하려고 할 때 버전 충돌 문제가 발생할 수 있습니다이 있습니다. 대신 Xamarin 또는.NET 응용 프로그램에서 사용할 수 있도록 API를 디자인 하려고 합니다.

### <a name="publishing-nuget-packages"></a>NuGet 패키지 게시

NuGet 패키지를 **nuspec** 파일은 프로젝트 부분 패키지의 게시 된 정의 하는 xml 파일입니다. 합니다 **nuspec** 파일 id, 제목, 작성자 등 패키지에 대 한 정보도 포함 됩니다.

참조 [NuGet의 설명서](/nuget/create-packages/creating-a-package.md) NuGet 패키지 만들기 및 게시 하는 방법에 대 한 자세한 내용은 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에 대 한 재사용 가능한 플러그 인 만들기](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Xamarin (비디오) 용 플러그 인을 사용 하 여 및 개발](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
