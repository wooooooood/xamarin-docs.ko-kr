---
title: 요약 9 장입니다. 플랫폼별 API 호출
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 9 장에서 요약 합니다. 플랫폼별 API 호출'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8a035da3dec468df291a19849ca89964c6707589
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994759"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>요약 9 장입니다. 플랫폼별 API 호출

플랫폼에 따라 달라 지는 일부 코드를 실행 하는 데 필요한 경우가 있습니다. 이 장에서 기술을 살펴봅니다.

## <a name="preprocessing-in-the-shared-asset-project"></a>공유 자산 프로젝트의 전처리

Xamarin.Forms 공유 자산 프로젝트는 C# 전처리기 지시문을 사용 하 여 각 플랫폼에 대 한 다른 코드를 실행할 수 있습니다 `#if`하십시오 `#elif`, 및 `endif`합니다. 이 확인할 [ **PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![변수의 세 번 스크린 샷 단락 서식이 지정 된](images/ch09fg01-small.png "운영 체제 및 장치 모델")](images/ch09fg01-large.png#lightbox "장치 모델 및 운영 체제")

그러나 결과 코드 보기 및 읽기 어려울 수 있습니다.

## <a name="parallel-classes-in-the-shared-asset-project"></a>공유 자산 프로젝트의 병렬 클래스

SAP의 플랫폼별 코드를 실행 하려면 더 구조화 된 접근법에 설명 되어는 [ **PlatInfoSap2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) 샘플입니다. 각 플랫폼 프로젝트는 동일 하 게 명명 된 클래스 및 메서드를 포함 하지만 해당 특정 플랫폼에 대 한 구현 합니다. SAP 다음 단순히 클래스를 인스턴스화하고 메서드를 호출 합니다.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService 및 이식 가능한 클래스 라이브러리

라이브러리는 응용 프로그램 프로젝트에서 클래스를 액세스할 일반적으로 수 없습니다. 설명한 기술을 사용 하지 않으려면이 제한 사항은 같습니다 **PlatInfoSap2** PCL에서 사용 되지 않도록 합니다. Xamarin.Forms에서 클래스를 포함 하는 반면 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) .NET 리플렉션을 PCL에서 응용 프로그램 프로젝트에서 공용 클래스에 액세스를 사용 하 여 합니다.

PCL을 정의 해야 합니다는 `interface` 각 플랫폼에서 사용 해야 하는 멤버를 사용 하 여 합니다. 그런 다음 플랫폼 중 각 해당 인터페이스의 구현을 포함합니다. 인터페이스를 구현 하는 클래스를 사용 하 여 식별 되어야 합니다는 [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute) 어셈블리 수준에 있습니다.

다음 사용 하 여 제네릭 PCL [ `Get` ](xref:Xamarin.Forms.DependencyService.Get*) 메서드의 `DependencyService` 인터페이스를 구현 하는 플랫폼 클래스의 인스턴스를 가져오려고 합니다.

에 설명 되어이 [ **DisplayPlatformInfo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) 샘플입니다.

## <a name="platform-specific-sound-generation"></a>플랫폼별 사운드 생성

[ **MonkeyTapWithSound** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) 샘플 경고음을 추가 합니다 **MonkeyTap** 각 플랫폼에서 소리 생성 기능에 액세스 하 여 프로그램입니다.



## <a name="related-links"></a>관련 링크

- [9 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [9 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
