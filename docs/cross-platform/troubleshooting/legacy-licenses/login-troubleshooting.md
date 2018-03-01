---
title: "Mac 용 Visual Studio 또는 Visual Studio에서 Xamarin에 로그인 이유 해야 수 없습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Mac 용 Visual Studio 또는 Visual Studio에서 Xamarin에 로그인 이유 해야 수 없습니다.

> [!IMPORTANT]
> 이 가이드를 소유 하거나 사용 하지 않는 Xamarin 계정에 로그인 하지 않아도 됩니다 때문에 대부분 MSDN 사용자에 게 적용 되지 않습니다는 [Xamarin 구성 요소를 저장](https://components.xamarin.com/) 또는 [Mac (Mac)에 대 한 Visual Studio](~/cross-platform/get-started/requirements.md)합니다. MSDN 라이선스 소유자에 게가를 참조 해야 [라이선스 옵션을 안내](~/cross-platform/get-started/requirements.md) 대신 합니다.



## <a name="overview"></a>개요
IDE에서 Xamarin 계정에 로그인 하면 못하게 할 수 있는 몇 가지 일반적인 원인은 있습니다. 알려진된 문제 및 수정 프로그램 아래에 설명 되어 있습니다.

### <a name="finding-the-login-screen"></a>로그인 화면 찾기

참조를 위해 로그인 화면을 여기 있습니다.

- Visual Studio for Mac
   - 시작 화면 오른쪽 위 모서리
   - **Mac 용 visual Studio > 계정** (Mac)
   - **도구 > 계정** (Windows)
- Visual Studio
   - **도구 > Xamarin 계정**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>IDE에 연결 하지만 계정 화면 올바른 로그인 정보 항목이 표시 되지 않습니다.

이 문제는 일반적으로 수동 라이선스 재 동기화 하면 해결 됩니다.
이 문서의 지침을 따르세요: [어떻게 할까요? 수동으로 resyncronize Xamarin 라이선스?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>잘못 된 계정 정보

Xamarin 웹 사이트로 이동 하는 경우 [로그인 페이지](https://store.xamarin.com/Login?from=%2faccount%2f), 현재 계정 자격 증명으로 로그인을 시도 수 있습니다.
페이지에는 다음과 같은 링크가 암호를 재설정 하 고 새 계정을 만들 수 있습니다.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>계정이 유효 하지만 IDE 연결할 수 없습니다.

이 대개 방화벽이 나 기타 보안 설정 IDE 필요한 끝점에 액세스 하지 못하도록 차단 하는 경우 발생 합니다.
정품 인증 서버에 다음 액세스 해야 합니다.

> activation.xamarin.com store.xamarin.com auth.xamarin.com

그러나 다른 여러 개의 끝점은 업데이트를 등 NuGet 패키지를 가져오는 등의 일반 개발 프로세스에 대 한 중요 합니다. 이 인해는 것이 좋습니다 되도록 *모든* 끝점에서 추가 되는 [Xamarin 방화벽 구성 가이드](~/cross-platform/get-started/installation/firewall.md)합니다.

### <a name="ios-in-xamarin-studio-windows"></a>Xamarin Studio 창에서 iOS
Windows 용 Xamarin Studio에서 iOS 개발 지원 되지 않습니다. 여기에 로그인 화면에 액세스할 때는 iOS 라이선스 표시 되지 않습니다.

대신, Visual Studio 또는 Xamarin Studio (Mac)을 통해 레거시 라이선스를 사용 하 여 로그인 합니다. MSDN 사용자 Windows에 로그인 할 필요 하지 않은 참고 합니다.

## <a name="additional-support"></a>추가 지원

위의 시나리오에 해당 상황에 설명 하지 않거나 문제 해결, 하는 경우 다음을 참조 하십시오 [지원 옵션](https://www.xamarin.com/support)합니다.
