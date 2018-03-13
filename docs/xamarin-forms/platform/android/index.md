---
title: "Android 플랫폼 기능"
description: "Xamarin.Forms 앱에 Android 관련 기능을 추가합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 9b5e9a4c449bc99bd88fc415f5ebb969d2c2a08a
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-features"></a>Android 플랫폼 기능

## <a name="platform-support"></a>플랫폼 지원

기본 Xamarin.Forms Android 프로젝트는 이전 스타일 공통적으로 Android 5.0 이전 컨트롤 렌더링을 사용 합니다. 서식 파일을 사용 하 여 빌드된 응용 프로그램에는 `FormsApplicationActivity` 자신의 기본 활동의 기본 클래스로 합니다.

## <a name="material-design-via-appcompat"></a>AppCompat 통해 자재 디자인

Xamarin.Forms에는 선택적 `FormsAppCompatActivity` 사용 하 여 **AppCompat** 자료 디자인 테마를 구현 하는 Android에서 제공 하는 기능입니다.

자료 디자인 테마 Xamarin.Forms Android 프로젝트에 추가 하려면 다음과 [AppCompat에 대 한 설치 지침 지원](appcompat.md)

다음은 **Todo** 기본 샘플 `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "AppCompat 없이 Todo 샘플 응용 프로그램")](images/before-appcompat.png#lightbox "AppCompat 없이 Todo 샘플 응용 프로그램")

이 동일한 코드를 사용 하도록 프로젝트를 업그레이드 한 후 및 `FormsAppCompatActivity` (및 추가 테마 정보를 추가):

[![](images/post-appcompat-sml.png "Todo 샘플 응용 프로그램을 AppCompat 및 테마")](images/post-appcompat.png#lightbox "AppCompat 및 테마 설정 Todo 샘플 응용 프로그램")

> [!NOTE]
> 사용 하는 경우 `FormsAppCompatActivity`, [일부 Android 사용자 지정 렌더러의 기본 클래스](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) 달라 집니다.


## <a name="related-links"></a>관련 링크

- [자재 디자인 지원 추가](appcompat.md)
