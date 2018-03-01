---
title: "요약 장 9입니다. 플랫폼별 API 호출"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 637096d3ebb7fb90321f7f459e0ca9e51572d935
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>요약 장 9입니다. 플랫폼별 API 호출

플랫폼에 따라 달라 지는 일부 코드를 실행 하는 경우가 가끔 있습니다. 이 장에서 기법을 탐색합니다.

## <a name="preprocessing-in-the-shared-asset-project"></a>공유 자산 프로젝트에서 전처리

Xamarin.Forms 공유 자산 프로젝트용 C# 전처리기 지시문을 사용 하 여 각 플랫폼에 대 한 다른 코드를 실행할 수 있으며 `#if`, `#elif`, 및 `endif`합니다. 이 확인할 [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![변수의 세 스크린 샷 단락 서식이 지정 된](images/ch09fg01-small.png "장치 모델 및 운영 체제")](images/ch09fg01-large.png "장치 모델 및 운영 체제")

그러나 결과 코드에 있으며 읽기 어려울 수 있습니다.

## <a name="parallel-classes-in-the-shared-asset-project"></a>공유 자산 프로젝트의 병렬 클래스

SAP의 플랫폼별 코드를 실행 하는 구조적된 더 접근 방식에 설명 된 [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) 샘플. 각 플랫폼 프로젝트는 동일 하 게 명명 된 클래스 및 메서드를 포함 하지만 해당 특정 플랫폼에 대 한 구현 합니다. SAP 다음 단순히 인스턴스화 클래스 및 메서드를 호출 합니다.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService 및 이식 가능한 클래스 라이브러리

라이브러리 응용 프로그램 프로젝트의 클래스에 정상적으로 액세스할 수 없습니다. 설명한 기술을 방지 하기 위해이 제한 사항은 같습니다 **PlatInfoSap2** PCL에 사용 되지 않도록 합니다. Xamarin.Forms 클래스를 포함 하는 반면 [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 하는.NET 리플렉션을 사용 하 여 PCL에서 응용 프로그램 프로젝트에서 공용 클래스에 액세스할 수 있습니다.

정의 해야 하는 PCL는 `interface` 각 플랫폼에서 사용 하는 데 필요한 멤버와 함께 합니다. 그런 다음 각각의 플랫폼 해당 인터페이스의 구현을 포함합니다. 인터페이스를 구현 하는 클래스를 식별 해야는 [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) 어셈블리 수준에 있습니다.

제네릭에 다음 사용 하 여 PCL [ `Get` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) 방식의 `DependencyService` 인터페이스를 구현 하는 플랫폼 클래스의 인스턴스를 가져오는 합니다.

이 확인할는 [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) 샘플.

## <a name="platform-specific-sound-generation"></a>플랫폼별 사운드 생성

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) 경고음을 추가 하는 예제는 **MonkeyTap** 각 플랫폼에서 소리 생성 기능에 액세스 하 여 프로그래밍 합니다.



## <a name="related-links"></a>관련 링크

- [9 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [9 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
