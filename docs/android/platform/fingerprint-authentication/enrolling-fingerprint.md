---
title: "지문 등록"
description: "화면 잠금 및 Android 장치 또는 에뮬레이터에 지문 등록 설정 하는 방법."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20e6d693f2a3eba54afaf1d3c7054ad75d7a7610
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="enrolling-a-fingerprint"></a>지문 등록

## <a name="enrolling-a-fingerprint-overview"></a>등록에 대 한 지문 개요

장치가 이미 구성 되었습니다. 지문 인증을 사용 하는 경우 지문 인증을 활용 하 여 Android 응용 프로그램은 있습니다. 이 가이드에는 Android 장치 또는 에뮬레이터에 지문을 등록 하는 방법을 설명 합니다. 에뮬레이터 지문 검색을 수행 하는 실제 하드웨어 없지만 Android 디버그 브리지 (아래 설명 참조)의 도움을 사용 하 여 지문 검사를 시뮬레이트할 수 있습니다.  이 가이드에는 Android 장치에서 화면 잠금 기능을 사용 하도록 설정 하 고 인증에 대 한 지문을 등록 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

지문을 등록 하려면 Android 장치 또는 API 수준 23 (Android 6.0)를 실행 하는 에뮬레이터 있어야 합니다.

Android 디버그 브리지 (ADB)를 사용 하 여 명령 프롬프트에 익숙한 필요 및 **adb** 실행 파일 Bash, PowerShell 경로 또는 명령 프롬프트 환경 해야 합니다.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>화면 잠금을 구성 하 고 지문 등록 

화면 잠금을 설정 하려면 다음 단계를 수행 합니다.

1. 로 이동 **설정 > 보안**를 선택 하 고 **화면 잠금**:

    ![보안 화면에서 화면 잠금 선택 항목의 위치](enrolling-fingerprint-images/testing-01.png)

2. 표시 된 다음 화면에서 선택 하 고 화면 잠금 보안 방법 중 하나를 구성할 수 있습니다. 

    ![살짝, 패턴, PIN 또는 암호를 선택 합니다.](enrolling-fingerprint-images/testing-02.png)

   선택 하 고 사용 가능한 화면 잠금 방법 중 하나를 완료 합니다.

3. screenlock 구성 되 면 돌아가려면는 **설정 > 보안** 페이지로 돌아간 후 선택 **지문**:

    ![보안 화면에서 지문 선택 항목의 위치](enrolling-fingerprint-images/testing-03.png)

4. 여기에서 장치에 지문을 추가 하는 순서를 수행 합니다.

    [![스크린 샷의 지문 장치를 추가 하기 위한 시퀀스](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 마지막 화면에서 손가락 지문 스캐너에 배치 하 라는 메시지가 표시 됩니다. 

    ![센서에 손가락을 배치 하 라는 메시지가 포함 된 화면](enrolling-fingerprint-images/testing-05.png)

    Android 장치를 사용 하는 경우 스캐너에 손가락으로 터치 하 여 프로세스를 완료 합니다. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>에뮬레이터에 대 한 지문 검색이 시뮬레이션

Android 에뮬레이터에서 Android 디버그 브리지를 사용 하 여 지문 검색을 시뮬레이션 하는 것이 같습니다. OS X 시작 시에 터미널 세션에서 Windows Powershell 세션 또는 명령 프롬프트를 시작 하 고 실행 `adb`:

```shell
$ adb -e emu finger touch 1
```

값 **1** 는 _손가락\_id_ 손가락 "검사"에 대 한 합니다. 각 가상 지문에 할당 하는 고유한 정수는 나중에 응용 프로그램을 실행 하면 수이 동일한 ADB를 실행할 명령 에뮬레이터에서 사용자가 묻는 메시지가 지문에 대해 실행할 수 있습니다는 `adb` 명령을 전달 하는 것은 _손가락\_id_ 지문 검사를 시뮬레이션할 수 .

지문 검색이 완료 되 면 Android에서 알려 지문을 추가 되었습니다.  

![추가 지문 표시 된 화면!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>요약 

이 가이드에는 화면 잠금을 설정 하 고 지문 Android 에뮬레이터 또는 Android 장치에 등록 하는 방법을 설명 합니다. 

