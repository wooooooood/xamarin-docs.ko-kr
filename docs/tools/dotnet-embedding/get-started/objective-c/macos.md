---
title: "MacOS 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 5e2f14e7b29f85e838563914089743f56239d7bb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-macos"></a>MacOS 시작


## <a name="what-you-will-need"></a>요구 사항

* 지침에 따라 우리의 [Objective-c 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드입니다.

* 사용 하면.NET 어셈블리 **Embeddinator 4000**합니다.

* MacOS Cocoa 응용 프로그램

지침에 따라 작업 한 후 계속 해 서 우리의 [Objective-c 시작](~/tools/dotnet-embedding/get-started/objective-c/index.md) 가이드입니다. .NET 어셈블리를 이미 있는 경우에 직접 건너뛸 수 있습니다 **를 사용 하 여 Embeddinator 4000** 섹션.

## <a name="creating-a-net-assembly"></a>.NET 어셈블리 만들기

열어야 할.NET 어셈블리를 빌드하려면 [Mac 용 Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) 을 새로 만듭니다 **.NET 라이브러리 프로젝트** 수행 하 여 *파일 > 새 솔루션 > 기타 >.NET > 라이브러리*. 다음을 클릭 하 고 제공 *날씨* 으로 *프로젝트 이름*에서 클릭 하 고 *만들기*합니다.

다음 단계를 따릅니다.

1. 삭제 된 **MyClass.cs** 파일 및 **속성** 폴더입니다.

2. 마우스 오른쪽 단추로 클릭 *날씨 프로젝트 > 추가 > 새 파일입니다.*

3. 선택 *빈 클래스* 사용 하 여 **XAMWeatherFetcher** 이름으로 새로 만들기를 클릭 합니다.

4. 내용을 대체 *XAMWeatherFetcher.cs* 를 다음 코드로:

```csharp
using System;
using System.Json;
using System.Net;

public class XAMWeatherFetcher {

    static string urlTemplate = @"https://query.yahooapis.com/v1/public/yql?q=select%20item.condition%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22{0}%2C%20{1}%22)&format=json&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys";
    public string City { get; private set; }
    public string State { get; private set; }

    public XAMWeatherFetcher (string city, string state)
    {
        City = city;
        State = state;
    }

    public XAMWeatherResult GetWeather ()
    {
        try {
            using (var wc = new WebClient ()) {
                var url = string.Format (urlTemplate, City, State);
                var str = wc.DownloadString (url);
                var json = JsonValue.Parse (str)["query"]["results"]["channel"]["item"]["condition"];
                var result = new XAMWeatherResult (json["temp"], json["text"]);
                return result;
            }
        }
        catch (Exception ex) {
            // Log some of the exception messages
            Console.WriteLine (ex.Message);
            Console.WriteLine (ex.InnerException?.Message);
            Console.WriteLine (ex.InnerException?.InnerException?.Message);

            return null;
        }

    }
}

public class XAMWeatherResult {
    public string Temp { get; private set; }
    public string Text { get; private set; }

    public XAMWeatherResult (string temp, string text)
    {
        Temp = temp;
        Text = text;
    }
}
```

한 `Using System.Json;` ;에서 다음을 수행 해야이 문제를 해결 하면 오류가 발생 합니다.

1. 두 번 클릭 하 고 **참조** 폴더입니다.

2. 클릭는 **패키지** 탭 합니다.

3. 확인 **System.Json**합니다.

4. Click **Ok**.

위의 작업이 완료 되 면 해야 할 모든은 빌드.NET 어셈블리를 클릭 하 여 *빌드 메뉴 > 모두 빌드* ⌘ + b 또는 합니다. 빌드 성공적 상위 상태 표시줄에 메시지가 표시 됩니다.

이제 마우스 오른쪽 단추로 클릭 *날씨* 프로젝트 노드를 선택 *Finder에 표시할*합니다. Finder에서 이동 하 여 *bin/Debug* 폴더; 내부 찾을 것 **Weather.dll 합니다.**

## <a name="using-embeddinator-4000"></a>Embeddinator 4000를 사용 하 여

Embeddinator 4000 pkg 설치를 사용 하 여를 설치 하 고 설치 후 새 터미널 세션 시작을 사용할 수 있습니다는 **objcgen** 명령 (그렇지 않으면 해당 절대 경로 사용할 수 있습니다: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`); **objcgen** 도구를 사용 하 여.NET 어셈블리의 네이티브 라이브러리를 생성 해야 합니다.

열기 터미널 `cd` Weather.dll, 포함 된 폴더에 및 실행 **objcgen** 아래에 표시 된 인수를 사용 합니다.

```shell
cd /Users/Alex/Projects/Weather/Weather/bin/Debug

objcgen --debug --outdir=output -c Weather.dll
```

필요한 배치 될에 모든 항목은 **출력** 옆에 디렉터리 *Weather.dll*합니다. 사용자 고유의.NET 어셈블리를 설정한 경우 대체 *Weather.dll* 것 및 위의 동일한 프로세스를 따릅니다.

## <a name="using-the-generated-output-in-an-xcode-project"></a>Xcode 프로젝트에서 생성 된 출력을 사용 하 여

Xcode를 열고 새 **macOS Cocoa 응용 프로그램** 하 고 이름을 **MyWeather**합니다. 마우스 오른쪽 단추로 클릭는 *MyWeather 프로젝트 노드*선택, *"MyWeather"에 파일 추가*로 이동는 **출력** 에서 만든 *Embeddinator 4000* , 다음 파일을 추가 합니다.

* bindings.h
* embeddinator.h
* glib.h
* mono-support.h
* mono_embeddinator.h
* objc-support.h
* libWeather.dylib
* Weather.dll

확인 **필요한 경우 항목을 복사할** 파일 대화 상자의 옵션 패널에서 선택 합니다.

다음 사항을 확인 해야 하는 이제 **libWeather.dylib** 및 **Weather.dll** 앱 번들에:

* 클릭 *MyWeather 프로젝트 노드*합니다.
* 선택 *빌드 단계* 탭 합니다.
* 새로 추가 *파일 복사 단계*합니다.
* *대상* 선택 **프레임 워크** 추가 **libWeather.dylib**합니다.
* 새로 추가 *파일 복사 단계*합니다.
* *대상* 선택 **실행 파일**, 추가 **Weather.dll** 있는지 확인 하 고 *복사본에서 코드 서명* 을 선택 합니다.

이제 열 **ViewController.m** 그 내용을 바꿉니다.

```objective-c
#import "ViewController.h"
#import "bindings.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    XAMWeatherFetcher * fetcher = [[XAMWeatherFetcher alloc] initWithCity:@"Boston" state:@"MA"];
    XAMWeatherResult * weather = [fetcher getWeather];

    NSString * result;
    if (weather)
        result = [NSString stringWithFormat:@"%@ °F - %@", weather.temp, weather.text];
    else
        result = @"An error occured";

    NSTextField * textField = [NSTextField labelWithString:result];
    [self.view addSubview:textField];
}

@end
```

마지막으로 Xcode 프로젝트를 실행 하 고 다음과 같이 표시 됩니다.

![MyWeather 샘플 실행](macos-images/weather-from-csharp-macos.png)

보다 완전 하 고 멋진 샘플은 사용할 수 있는 [여기](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather)합니다.
