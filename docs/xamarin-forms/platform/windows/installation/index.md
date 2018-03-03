---
title: "설치 Windows 프로젝트"
description: "기존 Xamarin.Forms 솔루션에 새 Windows 프로젝트를 추가합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 2acabc68c595bfb3bbd5c3516f68cd777ba10327
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="setup-windows-projects"></a>설치 Windows 프로젝트

_기존 Xamarin.Forms 솔루션에 새 Windows 프로젝트를 추가합니다._

이전 Xamarin.Forms 프로젝트 (또는 Mac OS에서 만든&nbsp;X)이 Windows 프로젝트 설정 수는 없습니다.

즉, Windows 8.1, Windows Phone 8.1 및 Windows 10 (UWP) 앱을 빌드 하려는 이러한 프로젝트 형식에 수동으로 추가할 필요 합니다.

## <a name="add-a-windows-81-app"></a>Windows 8.1 앱 추가

* PCL 서식 파일을 사용 하는 경우 [프로필 업데이트](#pcl), 한 다음
* [Windows 8.1 앱 추가](~/xamarin-forms/platform/windows/installation/tablet.md) 태블릿/데스크톱 폼 요소에 있습니다.

## <a name="add-a-windows-phone-81-app"></a>추가 된 Windows Phone 8.1 앱

* PCL 서식 파일을 사용 하는 경우 [프로필 업데이트](#pcl), 한 다음
* [추가 된 Windows Phone 8.1 앱](~/xamarin-forms/platform/windows/installation/phone.md)

## <a name="add-a-universal-windows-platform-uwp-app"></a>추가 유니버설 Windows 플랫폼 (UWP) 앱

* 빌드 [UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx) 앱 Windows 10에서 실행 되는 Visual Studio 2015 필요 합니다.
* PCL 서식 파일을 사용 하는 경우 [프로필 업데이트](#pcl), 한 다음
* [추가 유니버설 Windows 플랫폼 앱](~/xamarin-forms/platform/windows/installation/universal.md)

<a name="pcl" />

### <a name="update-your-pcl-profile"></a>PCL 프로필 업데이트

기존 Xamarin.Forms 앱 PCL 이식 가능한 클래스 라이브러리 () 템플릿을 사용 하는 경우 해당 프로필을 업데이트 해야 합니다.

1. **마우스 오른쪽 단추로 클릭 > 속성** (기존 설정이 다를 수 있습니다)

  ![](images/targets.png "PCL 대상")

2. 클릭는 **변경 중...**  단추

3. 확인 된 **Windows 8** 및 **Windows Phone 8.1** 옵션을 선택 (및 **Windows Phone Silveright** 은 *선택 취소*):

  ![](images/pcl.png "PCL 대상 옵션")

4. 키를 눌러 **확인** 변경 내용을 저장 합니다.

이 **프로필 111** 구성 하는 경우 프로그램 PCL Visual Studio에서 드롭 다운 목록을 사용 하 여 Mac에 대 한 합니다.

  ![](images/pcl-xs.png "PCL 프로필 111")

**참고:** 솔루션에 아직 Windows Phone 8 Silverlight 프로젝트는 PCL 프로필 259을 설정 해야 합니다. Windows Phone 8 Silverlight 지원 하므로이 페이지에 표시 되는 프로젝트 형식으로 바꾸는 것이 좋습니다. 되지 않고지 않습니다.
