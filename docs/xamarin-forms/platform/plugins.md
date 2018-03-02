---
title: "플러그 인"
description: "Xamarin.Forms 응용 프로그램에 기본 기능을 쉽게 추가합니다"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
ms.openlocfilehash: ad338e655c1aeb475122c837ca5c805e491f84bc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="plugins"></a>플러그 인

약간 다른 Api 설치 했지만 모든 플랫폼 간에 존재 하는 네이티브 플랫폼 기능이 많이 있습니다. 개발자는 다른 사용자와 공유할 수도 수 있는 이러한 기능에 대 한 추상 플랫폼 간 인터페이스를 만들려면 플러그 인을 작성 합니다.

이러한 기능에 포함: 배터리 상태, 나침반, 동작 센서, 지리적 위치, 텍스트 음성 변환, 및 더 많은 합니다. 이러한 기능에 쉽게 액세스 하 여 Xamarin.Forms 응용 프로그램을 허용 하는 플러그 인.

## <a name="finding-and-adding-plugins"></a>찾기 및 플러그 인 추가

Xamarin 커뮤니티 만들었습니다 대부분의 플랫폼 간 플러그 인-Xamarin.Forms 호환 큰 컬렉션에서 찾을 수 있습니다.

[**Xamarin 플러그 인**](https://github.com/xamarin/plugins)

프로젝트에 NuGet 패키지를 추가 하 여 지침에 대 한이 연습에서 참조 [프로젝트에서 NuGet 패키지를 포함 하 여](/visualstudio/mac/nuget-walkthrough/)합니다.


## <a name="creating-plugins"></a>플러그 인 만들기

만들고 Nuget 패키지 (및 Xamarin 구성 요소)와 플러그 인을 직접 게시할 수 이기도 합니다. 대부분의 기존 플러그 인 오픈 소스 되므로 writtern 된 방식을 이해 하는 코드를 검토할 수 있습니다.

예를 들어 플러그 인 아래 목록 모두 오픈 소스 이며의 샘플에 해당 하는 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) 섹션:

- **텍스트 음성 변환** James Montemagno 여 &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) 및 [NuGet  ](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **배터리 상태** James Montemagno 여 &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) 및 [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

에 대 한 이러한 지침을 수행 하는 대로 해당 Github 프로젝트 플랫폼 간 플러그 인을 직접를 만들기 위한 좋은 출발점 제공할 수 [Xamarin에 대 한 플러그 인 만들기](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin)합니다.

### <a name="structuring-cross-platform-plugin-projects"></a>플랫폼 간 플러그 인 프로젝트를 구성합니다.

NuGet 패키지를 디자인 하기 위한 특정 요구에는 없지만은 플랫폼 간 앱에 대 한 패키지를 만들기 위한 몇 가지 지침입니다.

플랫폼 간 플러그 인 해야 다음 구성 요소가 일반적으로 구성 됩니다.

- PCL는 플러그 인에 대 한 API를 나타내는 인터페이스
- iOS, Android 및 Windows 인터페이스의 구현으로 라이브러리 클래스입니다.

읽기 James Montemagno의 [블로그 게시물](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/) Xamarin에 대 한 플러그 인을 만드는 과정을 설명 하는 합니다.

Xamarin.Forms는 플러그 인에서 직접 참조 하지는 보다 선호 됩니다.
이 다른 개발자가 플러그 인을 사용 하려고 할 때 버전 충돌 문제가 발생할 수 있습니다. 대신 Xamarin 또는.NET 응용 프로그램에서 사용할 수 있도록 API를 디자인 하려고 합니다.

### <a name="publishing-nuget-packages"></a>NuGet 패키지를 게시합니다.

NuGet 패키지는 **nuspec** 파일은 프로젝트의 부분 패키지에 게시를 정의 하는 xml 파일입니다. **nuspec** 파일 id, 제목 및 작성자와 같은 패키지에 대 한 정보도 포함 됩니다.

참조 [NuGet의 설명서](http://docs.nuget.org/create/creating-and-publishing-a-package) 만들기 및 NuGet 패키지를 게시 하는 방법에 대 한 자세한 내용은 합니다.


## <a name="related-links"></a>관련 링크

- [Xamarin.Forms에 대 한 다시 사용할 수 있는 플러그 인 만들기](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Xamarin (비디오)에 대 한 개발 및 사용 하 여 플러그 인](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
