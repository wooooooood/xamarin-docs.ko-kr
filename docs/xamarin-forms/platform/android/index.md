---
title: Android 플랫폼 기능
description: 이 문서에서는 안드로이드에만 적용되는 기능을 Xamarin.Forms 앱에 추가하는 방법과, 머티리얼 디자인에 대해 설명하는 데에 초점을 둡니다.
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 2eada518586f222d200ec19aeddc65107d7603b3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/01/2018
ms.locfileid: "35242406"
---
# <a name="android-platform-features"></a>Android 플랫폼 기능

## <a name="platform-support"></a>플랫폼 지원

원래는 기본 Xamarin.Forms Android 프로젝트는 Android 5.0 전까지 가장 많이 쓰이는 옛날 스타일의 컨트롤 렌더링을 사용했습니다. 이 템플릿을 가지고 만들어진 앱은 `FormsApplicationActivity`를 메인 액티비티의 베이스 클래스로 가지고 있습니다.

## <a name="material-design-via-appcompat"></a>AppCompat을 이용한 머티리얼 디자인

Xamarin.Forms 안드로이드 프로젝트들은 이제 `FormsAppCompatActivity`를 메인 엑티비티의 베이스 클래스로 쓰고 있습니다. 이 클래스는 Android로 부터 제공된 **AppCompat**의 기능들을 사용하여 머티리얼 디자인 테마를 구현합니다.

머티리얼 디자인 테마를 자신의 Xamarin.Forms 안드로이드 프로젝트에 추가하려면 [AppCompat 지원에 대한 설치 지침](appcompat.md)을 참고하세요.

다음은 기본 `FormsApplicationActivity`를 포함한 **Todo** 샘플입니다.

[![](images/before-appcompat-sml.png "AppCompat을 사용하지 않은 Todo 샘플 앱")](images/before-appcompat.png#lightbox "AppCompat 없이 Todo 샘플 응용 프로그램")

그리고 다음은 동일한 코드를 `FormsAppCompatActivity`를 사용하게 하고, 추가 테마 정보를 추가하여 프로젝트를 업그레이드한 결과입니다.

[![](images/post-appcompat-sml.png "AppCompat과 테마를 사용한 Todo 샘플 앱")](images/post-appcompat.png#lightbox "AppCompat 및 테마 설정 Todo 샘플 응용 프로그램")

> [!NOTE]
> `FormsAppCompatActivity`를 사용하는 경우, [몇몇 안드로이드 커스텀 랜더러들의 베이스 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)가 달라질 수 있습니다.


## <a name="related-links"></a>관련 링크

- [머티리얼 디자인 지원 추가하기](appcompat.md)
