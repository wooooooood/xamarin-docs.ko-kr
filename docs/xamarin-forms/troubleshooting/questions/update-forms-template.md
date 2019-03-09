---
title: Xamarin.Forms 기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있습니까?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: e439d39dd8591cad14485e64aabab2d6016a8e27
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668229"
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms 기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있습니까?

이 가이드에서는 예를 들어, Xamarin.Forms.NET Standard 라이브러리 템플릿을 사용 하지만 동일한 일반 메서드는 Xamarin.Forms 공유 프로젝트 템플릿에 대해 에서도 작동 합니다. Xamarin.Forms 2.1.0.6529 1.5.1.6471에서에서 업데이트 하는 예제를 사용 하 여이 가이드는 되지만 동일한 단계를 기본적으로 다른 버전을 대신 설정할 수 있습니다.

1.  원래 템플릿을 복사 `.zip` 에서:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  압축을 풉니다는 `.zip` 임시 위치에 있습니다.

3.  사용 하려는 새 버전으로 폼 패키지의 이전 버전의 모든 문자열을 변경 합니다.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    예: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  기본 요소의 "name"을 변경할 [다중 프로젝트 템플릿 파일](https://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) 고유 하 게 합니다. 예를 들어:
    > <Name>비어 있는 앱 (Xamarin.Forms 이식 가능)-2.1.0.6529</Name>

5.  전체 템플릿 폴더를 다시 압축 합니다. 원래 파일 구조와 일치 해야 합니다 `.zip` 파일입니다. `Xamarin.Forms.PCL.vstemplate` 파일의 맨 위에 있는 해야는 `.zip` 폴더 내 파일입니다.

6.  사용자별으로 Visual Studio 템플릿 폴더에 "모바일 앱" 하위 디렉터리를 만듭니다.
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  새 "Mobile Apps" 디렉터리에 새 압축 접속 템플릿 폴더를 복사 합니다.

8.  3 단계에서 버전과 일치 하는 NuGet 패키지를 다운로드 합니다. 예를 들어 [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (참고 [ https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), Xamarin Visual Studio 확장 폴더의 해당 하위 폴더에 복사 합니다.
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
