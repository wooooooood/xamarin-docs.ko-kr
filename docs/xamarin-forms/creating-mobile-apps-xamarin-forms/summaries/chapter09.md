---
title: 요약 - 9장. 플랫폼별 API 호출
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 9장. 플랫폼별 API 호출'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 4FFA1BD4-B3ED-461C-9B00-06ABF70D471D
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 3aec84ec6598a45bb989d4bbc1705fd797382755
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334559"
---
# <a name="summary-of-chapter-9-platform-specific-api-calls"></a>요약 - 9장. 플랫폼별 API 호출

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)

> [!NOTE] 
> 이 페이지의 정보는 Xamarin.Forms가 책에 제공된 자료와 다른 영역을 표시합니다.

플랫폼에 따라 달라지는 일부 코드를 실행해야 하는 경우도 있습니다. 이 장에서는 기법에 대해 살펴봅니다.

## <a name="preprocessing-in-the-shared-asset-project"></a>공유 자산 프로젝트에서 전처리

Xamarin.Forms 공유 자산 프로젝트는 C# 전처리기 지시문 `#if`, `#elif` 및 `endif`를 사용하여 각 플랫폼마다 다른 코드를 실행할 수 있습니다. 이는 [**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1)에 설명되어 있습니다.

[![가변 형식 단락의 삼중 스크린샷](images/ch09fg01-small.png "디바이스 모델 및 운영 체제")](images/ch09fg01-large.png#lightbox "디바이스 모델 및 운영 체제")

그러나 결과 코드는 복잡하고 읽기 힘들 수 있습니다.

## <a name="parallel-classes-in-the-shared-asset-project"></a>공유 자산 프로젝트의 병렬 클래스

SAP에서 플랫폼별 코드를 실행하는 보다 구조화된 접근 방식은 [**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) 샘플에 설명되어 있습니다. 각 플랫폼 프로젝트는 동일하게 명명된 클래스 및 메서드가 있지만 특정 플랫폼에 대해 구현됩니다. 그런 다음 SAP는 단순히 클래스를 인스턴스화하고 메서드를 호출합니다.

## <a name="dependencyservice-and-the-portable-class-library"></a>DependencyService 및 이식 가능한 클래스 라이브러리

> [!NOTE] 
> 이식 가능한 클래스 라이브러리는 .NET Standard 라이브러리로 대체되었습니다. 이 책의 모든 샘플 코드는 .NET Standard 라이브러리를 사용하도록 변환되었습니다.

라이브러리는 일반적으로 애플리케이션 프로젝트의 클래스에 액세스할 수 없습니다. 이 제한은 **PlatInfoSap2**에 표시된 기법이 라이브러리에서 사용되지 않도록 하는 것처럼 보입니다. 그러나 Xamarin.Forms에는 .NET 리플렉션을 사용하여 라이브러리에서 애플리케이션 프로젝트의 공용 클래스에 액세스하는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)라는 클래스가 있습니다.

라이브러리는 각 플랫폼에서 사용해야 하는 멤버를 사용하여 `interface`를 정의해야 합니다. 그런 다음 각 플랫폼에 해당 인터페이스의 구현이 포함됩니다. 인터페이스를 구현하는 클래스는 어셈블리 수준에서 [DependencyAttribute](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 식별해야 합니다.

그런 다음 라이브러리는 `DependencyService`의 제네릭 [`Get`](xref:Xamarin.Forms.DependencyService.Get*) 메서드를 사용하여 인터페이스를 구현하는 플랫폼 클래스의 인스턴스를 가져옵니다.

이는 [**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) 샘플에 설명되어 있습니다.

## <a name="platform-specific-sound-generation"></a>플랫폼별 소리 생성

[**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) 샘플은 각 플랫폼에서 소리 생성 기능에 액세스하여 **MonkeyTap** 프로그램에 경고음을 추가합니다.

## <a name="related-links"></a>관련 링크

- [9장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [9장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [종속성 서비스](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
