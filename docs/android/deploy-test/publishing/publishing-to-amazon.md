---
title: Amazon App Store에 게시
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 4805b9685328c5848bcdeab6a0fbaa0c5ed67844
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30762528"
---
# <a name="publishing-to-the-amazon-app-store"></a>Amazon App Store에 게시

Amazon Mobile App Distribution Program을 사용하면 모바일 앱 개발자들이 응용 프로그램을 Amazon에 게시할 수 있습니다. 이 섹션에서는 Amazon App Store for Android에 대해 간략히 설명합니다. 

[![Amazon App Store 화면](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png#lightbox)

Amazon은 APK의 크기를 제한하지 않습니다. 그러나 APK가 30MB를 초과하면 Amazon 모바일 앱 배포 포털이 아닌 FTP를 배포에 사용하게 됩니다.


## <a name="submitting-apps-binary-info"></a>앱 제출: 이진 정보

Amazon App Store에 응용 프로그램을 제출하는 것은 Google Play에 응용 프로그램을 제출하는 것과 유사한 프로세스입니다. Amazon에서 배포된 응용 프로그램에는 다음 자산이 필요합니다. 

-   **아이콘** &ndash; 투명한 배경의 114x114 .png 파일입니다. 필수입니다.
-   **축소판 그림** &ndash; 위 아이콘의 더 큰 버전입니다. 투명한 배경의 512 x 512 픽셀입니다. 이 아이콘도 필수입니다.
-   **스크린 샷** &ndash; Amazon에서는 최소 3개, 최대 10개의 스크린 샷을 요구합니다. 스크린샷은 1024w x 600h 픽셀 또는 800w x 480h 픽셀이어야 합니다. .png 및 .jpg 형식 모두 사용할 수 있습니다.
-   **프로모션 이미지** &ndash; 응용 프로그램이 홈 페이지 같은 프로모션 위치에 표시되려면 프로모션 이미지를 선택적으로 제출해야 할 수 있습니다. 이 이미지는 1024w x 500h 픽셀 .png 또는 .jpg 파일로, 가로 방향이어야 합니다. 애니메이션은 포함되면 안 됩니다.
-  5개 비디오에 대한 업데이트가 제공될 수 잇습니다.



## <a name="approval-process"></a>승인 프로세스

응용 프로그램이 제출되면 승인 프로세스를 거치게 됩니다.
Amazon은 응용 프로그램이 제품 설명 범위에서 작동하며 고객의 데이터에 유해하지 않고 장치 작동을 저해하지 않음을 검토하게 됩니다. 승인 프로세스가 완료되면 Amazon이 알림을 보내고 응용 프로그램을 배포합니다.
