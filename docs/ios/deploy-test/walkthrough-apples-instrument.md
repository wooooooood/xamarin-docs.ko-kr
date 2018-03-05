---
title: "연습 - Apple의 계측 도구 사용"
description: "이 문서에서는 Apple의 계측 도구를 사용하여 Xamarin으로 빌드된 iOS 응용 프로그램의 메모리 문제를 진단하는 방법을 설명합니다. 계측을 시작하고, 힙 스냅숏을 만들고, 메모리 증가를 분석하는 방법을 보여 줍니다. 또한 계측을 사용하여 메모리 문제가 발생하는 정확한 코드 줄을 표시하여 찾아내는 방법을 보여 줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 949a2ea4d5838b664e19251e69a32efccc98d496
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-apples-instruments-tool"></a>연습 - Apple의 계측 도구 사용

_이 문서에서는 Apple의 계측 도구를 사용하여 Xamarin으로 빌드된 iOS 응용 프로그램의 메모리 문제를 진단하는 방법을 설명합니다. 계측을 시작하고, 힙 스냅숏을 만들고, 메모리 증가를 분석하는 방법을 보여 줍니다. 또한 계측을 사용하여 메모리 문제가 발생하는 정확한 코드 줄을 표시하여 찾아내는 방법을 보여 줍니다._

이 페이지에서는 **Xcode의 계측 도구**를 사용하여 iOS 응용 프로그램의 메모리 문제를 진단하는 방법을 보여 줍니다.
먼저 Mac용 Visual Studio에서 [MemoryDemo 샘플](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)을 다운로드하고 **이** 솔루션을 엽니다.

## <a name="diagnosing-the-memory-issues"></a>메모리 문제 진단

1.  Mac용 Visual Studio에서 **도구 > 계측 시작** 메뉴 항목에서 **계측**을 시작합니다.
2.  **실행 > 장치에 업로드** 메뉴 항목을 선택하여 응용 프로그램을 장치에 업로드합니다.
3.  **할당** 템플릿(흰색 상자가 있는 주황색 아이콘)을 선택합니다.

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "할당 템플릿 선택")

4.  창 위쪽의 **프로비전 템플릿 선택:** 목록에서 **MemoryDemo** 응용 프로그램을 선택합니다. iOS 장치를 먼저 클릭하여 설치된 응용 프로그램을 보여 주는 메뉴를 펼칩니다.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "MemoryDemo 응용 프로그램 선택")

5.  **선택**(창 오른쪽 아래) 단추를 눌러 **계측**을 시작합니다. 이 템플릿은 위쪽 창에 두 개의 항목(할당 및 VM 추적기)을 표시합니다.

6.  계측의 **레코드** 단추(왼쪽 위의 빨간색 원)를 누릅니다. 그러면 응용 프로그램이 시작됩니다.

7.  위쪽 창에서 **VM 추적기** 행을 선택합니다(현재 앱이 실행되고 있으므로 Dirty(더티) 크기 및 Resident(설정) 크기의 두 섹션이 포함됨). **검사기** 창에서 **디스플레이 설정 표시** 옵션(기어 아이콘)을 선택한 다음, 이 스크린샷의 오른쪽 아래에 표시된 **자동 스냅숏** 확인란을 선택합니다.

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "디스플레이 설정 표시 옵션(기어 아이콘)을 선택한 다음, 자동 스냅숏 확인란을 선택함")

8.  위쪽 창에서 **할당** 행을 선택합니다(현재 앱이 실행되고 있으므로 *모든 힙 및 익명 VM*이라고 표시됨).
9.  **검사기** 창에서 **디스플레이 설정 표시** 옵션(기어 아이콘)을 선택한 다음, **표시 생성** 단추를 클릭하여 기준선을 설정합니다. 작은 빨간색 플래그가 창 위쪽의 타임라인에 표시됩니다.
10.  응용 프로그램을 스크롤한 다음, **표시 생성**을 다시 선택합니다(여러 번 반복).
11.  **중지** 단추를 클릭합니다.
12.  가장 큰 **증가**가 있는 **생성** 노드를 펼치고 **증가**(내림차순) 기준으로 정렬합니다.
13.  **스택 추적**을 보여 주는 **확장된 세부 정보 표시**("E")로 **검사기** 창을 변경합니다.

14.  **<non-object>** 노드에서 과도한 메모리 증가를 보여 줍니다. 이 노드 옆에 있는 화살표를 클릭하면 세부 정보가 표시됩니다. 스택 추적을 마우스 오른쪽 단추로 클릭하여 창에 **원본 위치**를 추가합니다.

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "창에 원본 위치 추가")

15.  **크기** 기준으로 정렬하고 **확장된 세부 정보** 보기를 표시합니다.

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "크기별 정렬 및 확장된 세부 정보 보기 표시")

16.  호출 스택에서 원하는 항목을 클릭하면 관련 코드가 표시됩니다.

    ![](walkthrough-apples-instrument-images/05-related-code.png "관련된 코드 보기")

이 경우 새 이미지가 만들어져 각 셀의 컬렉션에 저장되거나 기존 컬렉션 뷰 셀이 다시 사용되지 않습니다.

## <a name="resolving-the-memory-issues"></a>메모리 문제 해결

이러한 문제를 해결하고 계측을 통해 응용 프로그램을 다시 실행할 수 있습니다.

클래스 수준에서 단일 인스턴스를 선언하면, 다음과 같이 이미지를 다시 사용할 수 있으며 셀 개체를 매번 만들지 않는 대신 기존 풀에서 다시 사용할 수 있습니다.

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

이제 응용 프로그램을 실행하면 메모리 사용량이 크게 줄어듭니다. 생성 간의 **증가**는 이제 코드를 수정하기 전의 MiB(메가바이트) 대신 Kib(킬로바이트) 단위로 측정됩니다.

![](walkthrough-apples-instrument-images/06-reduced-memory.png "앱 메모리 사용량 표시")

향상된 코드는 Mac용 Visual Studio에서 **다음** 솔루션의 [MemoryDemo 샘플](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)에서 사용할 수 있습니다.

[Xamarin.iOS 가비지 수집](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)에 대한 이 커뮤니티 블로그는 Xamarin.iOS와 관련된 메모리 문제를 처리하는 데 유용한 참고 자료입니다.


## <a name="summary"></a>요약

이 문서에서는 계측을 사용하여 메모리 문제를 진단하는 방법을 설명했습니다.
Mac용 Visual Studio 내에서 계측을 시작하고, 메모리 할당 템플릿을 로드하고, 스냅숏을 사용하여 메모리 문제를 찾아내는 방법을 설명했습니다.
마지막으로, 응용 프로그램을 다시 검사하여 문제가 해결되었는지 확인했습니다.


## <a name="related-links"></a>관련 링크

- [MemoryDemo 샘플](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS 가비지 수집](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
