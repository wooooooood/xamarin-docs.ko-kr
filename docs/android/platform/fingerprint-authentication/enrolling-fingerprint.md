---
title: 지문 등록
description: Android 장치 또는 에뮬레이터에서 화면 잠금을 설정 하 고 지문을 등록 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: f52be16a81f3c8047997e1f4a88e13f6b940db14
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756414"
---
# <a name="enrolling-a-fingerprint"></a>지문 등록

## <a name="enrolling-a-fingerprint-overview"></a>지문 등록 개요

장치가 이미 지문 인증을 사용 하 여 구성 된 경우에만 Android 응용 프로그램에서 지문 인증을 활용할 수 있습니다. 이 가이드에서는 Android 장치 또는 에뮬레이터에 지문을 등록 하는 방법에 대해 설명 합니다. 에뮬레이터는 실제 하드웨어를 사용 하 여 지문 검사를 수행 하지 않지만 Android Debug Bridge의 도움을 사용 하 여 지문 검색을 시뮬레이션할 수 있습니다 (아래 설명 참조).  이 가이드에서는 Android 장치에서 화면 잠금을 사용 하도록 설정 하 고 인증을 위해 지문을 등록 하는 방법에 대해 설명 합니다.

## <a name="requirements"></a>요구 사항

지문을 등록 하려면 Android 장치 또는 API 레벨 23 (Android 6.0)을 실행 하는 에뮬레이터가 있어야 합니다.

ADB (Android Debug Bridge)를 사용 하려면 명령 프롬프트에 대해 잘 알고 있어야 하며, **ADB** 실행 파일은 Bash, PowerShell 또는 명령 프롬프트 환경의 경로에 있어야 합니다.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>화면 잠금 구성 및 지문 등록 

화면 잠금을 설정 하려면 다음 단계를 수행 합니다.

1. **설정 > 보안**으로 이동 하 고 **화면 잠금**을 선택 합니다.

    ![보안 화면에서 화면 잠금 선택 항목의 위치](enrolling-fingerprint-images/testing-01.png)

2. 표시 되는 다음 화면에서 화면 잠금 보안 방법 중 하나를 선택 하 고 구성할 수 있습니다. 

    ![살짝 밀기, 패턴, 핀 또는 암호를 선택 합니다.](enrolling-fingerprint-images/testing-02.png)

   사용 가능한 화면 잠금 방법 중 하나를 선택 하 여 완료 합니다.

3. Screenlock이 구성 되 면 **설정 > 보안** 페이지로 돌아가 **지문**을 선택 합니다.

    ![보안 화면에서 지문 선택의 위치](enrolling-fingerprint-images/testing-03.png)

4. 여기에서 시퀀스를 따라 장치에 지문을 추가 합니다.

    [![장치에 지문을 추가 하기 위한 스크린샷 시퀀스](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. 최종 화면에서 지문 스캐너에 손가락을 입력 하 라는 메시지가 표시 됩니다. 

    ![센서에 손가락을 입력 하 라는 메시지를 표시 하는 화면](enrolling-fingerprint-images/testing-05.png)

    Android 장치를 사용 하는 경우 스캐너에 손가락을 접촉 하 여 프로세스를 완료 합니다. 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>에뮬레이터에서 지문 스캔 시뮬레이션

Android 에뮬레이터에서 Android Debug Bridge를 사용 하 여 지문 검색을 시뮬레이션할 수 있습니다. OS X에서 터미널 세션을 시작 하는 동안 Windows에서 명령 프롬프트 또는 Powershell 세션을 시작 하 `adb`고 다음을 실행 합니다.

```shell
$ adb -e emu finger touch 1
```

값 **1** 은 "검사 됨" 인 핑거의 _핑거\_id_ 입니다. 각 가상 지 문으로 할당 하는 고유한 정수입니다. 나중에 앱이 실행 되는 경우 에뮬레이터에서 지문을 입력 하 라는 메시지가 표시 될 때마다 동일한 adb 명령을 실행할 수 있습니다. 그런 다음 `adb` 명령을 실행 하 고 지문 검색을 시뮬레이트하는 _손가락\_id_ 를 전달할 수 있습니다.

지문 검사가 완료 된 후 Android는 지문을 추가 했음을 알립니다.  

![추가 된 지문을 표시 하는 화면](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>요약 

이 가이드에서는 Android 장치 또는 Android 에뮬레이터에서 화면 잠금을 설정 하 고 지문을 등록 하는 방법에 대해 설명 했습니다. 
