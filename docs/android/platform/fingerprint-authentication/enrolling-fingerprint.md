---
title: 지문 등록
description: 설정 하는 방법을 화면 잠금 및 Android 장치 또는 에뮬레이터에서 지문 등록 합니다.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 18903a7d8f6c4033dc3ac7c4c0187a247d023bc1
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115915"
---
# <a name="enrolling-a-fingerprint"></a>지문 등록

## <a name="enrolling-a-fingerprint-overview"></a>등록에 대 한 지문 개요

장치 지문 인증을 사용 하 여 이미 구성 된 경우 지문 인증을 활용 하 여 Android 응용 프로그램에 대 한만 가능 합니다. 이 가이드에서는 Android 장치 또는 에뮬레이터에서 지문 등록 하는 방법을 설명 합니다. 에뮬레이터에 지문을 검색을 수행 하는 실제 하드웨어 없지만 (아래 설명 참조) Android 디버그 브리지를 사용 하 여 지문 검색을 시뮬레이션 하는 것이 가능 합니다.  이 가이드에서는 Android 장치에서 화면 잠금을 사용 하도록 설정 하 고 인증에 대 한 지문 등록 하는 방법을 설명 합니다.

## <a name="requirements"></a>요구 사항

지문을 등록 하려면 Android 장치 또는 에뮬레이터 API 레벨 23 (Android 6.0)를 실행 해야 합니다.

디버그 브리지 ADB (Android)를 사용 하 여 명령 프롬프트를 사용 하 여 필요 하며 **adb** 실행 파일의 Bash, PowerShell 경로 또는 명령 프롬프트 환경 해야 합니다.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>화면 잠금 기능을 구성 하 고 지문 등록 

화면 잠금에 설정 하려면 다음 단계를 수행 합니다.

1. 로 이동 **설정 > 보안**, 선택한 **화면 잠금**:

    ![보안 화면에서 화면 잠금 선택의 위치](enrolling-fingerprint-images/testing-01.png)

2. 표시 되는 다음 화면에 선택 하 고 화면 잠금 보안 방법 중 하나를 구성 하면: 

    ![안쪽으로 살짝 밀어, 패턴, PIN 또는 암호를 선택 합니다.](enrolling-fingerprint-images/testing-02.png)

   선택 하 고 사용 가능한 화면 잠금 방법 중 하나를 완료 합니다.

3. screenlock를 구성한 후에 반환 된 **설정 > 보안** 페이지를 선택 **지문**:

    ![보안 화면에서 지문 선택 항목의 위치](enrolling-fingerprint-images/testing-03.png)

4. 여기에서 장치에 지문을 추가 하는 순서를 수행 합니다.

    [![일련의 장치에 지문을 추가 하는 것에 대 한 스크린샷은](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 마지막 화면에서 손가락을 지문 스캐너를 배치할 것인지 묻는 메시지가 나타납니다. 

    ![센서에서 손가락을 배치 하는 화면](enrolling-fingerprint-images/testing-05.png)

    Android 장치를 사용 하는 경우 스캐너에 손가락을 터치 하 여 프로세스를 완료 합니다. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>에뮬레이터에서 지문 검색을 시뮬레이션합니다.

Android 에뮬레이터에서 Android 디버그 브리지를 사용 하 여 지문 검색을 시뮬레이션 하는 것이 같습니다. OS X의 터미널 세션에서 Windows 명령 프롬프트 또는 Powershell 세션을 시작 하 고 실행 `adb`:

```shell
$ adb -e emu finger touch 1
```

값 **1** 되는 _손가락\_id_ 손가락 "검사"에 대 한 합니다. 각 가상 지문에 할당 하는 고유한 정수 이며 응용 프로그램을 실행 하는 경우 나중에 수 실행이 동일한 ADB 명령은 에뮬레이터 때마다 묻는 지문에 대해 실행할 수 있습니다는 `adb` 명령을 전달 하는 _손가락\_id_ 지문 검색을 시뮬레이션 하기 .

지문 검사를 완료 한 후 Android 알려 지문을 추가 되었습니다.  

![화면 추가 지문 표시.](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>요약 

이 가이드에는 화면 잠금을 설정 하 고 Android 에뮬레이터 또는 Android 장치에서 지문 등록 하는 방법을 설명 합니다. 

