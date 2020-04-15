---
title: Xamarin.Android 조각 연습 - 1부
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 043ad02f9ca9148910364ac82917551ee58d72ba
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027398"
---
# <a name="fragments-walkthrough-ndash-phone"></a>조각 연습 &ndash; 휴대폰

이 연습은 가로 방향의 Android 디바이스를 대상으로 Xamarin.Android 앱을 만들기 위한 연습의 첫 번째 부분입니다. 이 연습에서는 Xamarin.Android에서 조각을 만드는 방법과 이를 샘플에 추가하는 방법을 설명합니다.

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

이 앱에 대해서는 다음 클래스가 생성됩니다.

1. `PlayQuoteFragment` &nbsp; 이 조각은 윌리엄 세익스피어의 희곡 중에서 하나의 인용구를 표시합니다. 이 조각은 `PlayQuoteActivity`에 의해 호스트됩니다.
1. `Shakespeare` &nbsp; 이 클래스는 2개의 하드 코딩된 배열을 속성으로 저장합니다.
1. `TitlesFragment` &nbsp; 이 조각은 윌리엄 세익스피어가 쓴 희곡의 제목 목록을 표시합니다. 이 조각은 `MainActivity`에 의해 호스트됩니다.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment`는 `TitlesFragment`에서 사용자의 희곡 선택에 대한 응답으로 `PlayQuoteActivity`를 시작합니다.

## <a name="1-create-the-android-project"></a>1. Android 프로젝트 만들기

**FragmentSample**이라는 새로운 Xamarin.Android 프로젝트를 만듭니다.
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![새로운 Xamarin.Android 프로젝트 만들기](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![새로운 Xamarin.Android 프로젝트 만들기](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

이 연습에서는 **최신 개발**을 선택하는 것이 좋습니다.

프로젝트를 만든 후에는 **layout/Main.axml** 파일 이름을 **layout/activity_main.axml**로 바꿉니다.

-----

## <a name="2-add-the-data"></a>2. 데이터 추가

이 애플리케이션의 데이터는 클래스 이름 `Shakespeare`의 속성인 2개의 하드 코딩된 문자열 배열에 저장됩니다.

* `Shakespeare.Titles` &nbsp; 이 배열에는 윌리엄 세익스피어의 희곡 목록이 저장됩니다. 이것은 `TitlesFragment`의 데이터 원본입니다.
* `Shakespeare.Dialogue` &nbsp; 이 배열에는 `Shakespeare.Titles`에 포함된 희곡 중 하나에서 가져온 인용구 목록이 저장됩니다. 이것은 `PlayQuoteFragment`의 데이터 원본입니다.

새 C# 클래스를 **FragmentSample** 프로젝트에 추가하고 이름을 **Shakespeare.cs**로 지정합니다. 이 파일 내에서 다음 콘텐츠가 포함된 `Shakespeare`라는 새로운 C# 클래스를 만듭니다.

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!"
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3. PlayQuoteFragment 만들기

`PlayQuoteFragment`는 애플리케이션에서 사용자가 전에 선택한 세익스피어 희곡의 인용구를 표시하는 Android 조각입니다. 이 조각은 Android 레이아웃 파일을 사용하지 않으며, 대신 사용자 인터페이스를 동적으로 만듭니다. `PlayQuoteFragment`라는 새 `Fragment` 클래스를 프로젝트에 추가합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![새 C# 클래스 추가](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![새 C# 클래스 추가](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

그런 후, 다음 코드 조각과 비슷하게 조각의 코드를 변경합니다.

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

이것은 Android에서 조각을 인스턴스화하는 팩터리 메서드를 제공하기 위한 일반적인 패턴입니다. 이렇게 하면 적절한 작동을 위해 필요한 매개 변수가 포함된 상태로 조각이 생성됩니다. 이 연습에서는 인용구가 선택될 때마다 새 조각을 만들기 위해 앱에서 `PlayQuoteFragment.NewInstance` 메서드가 사용됩니다. `NewInstance` 메서드는 표시할 인용구의 인덱스로 단일 매개 변수를 사용합니다.

`OnCreateView` 메서드는 화면에 조각을 렌더링할 때 Android에서 호출됩니다. 이 메서드는 조각인 Android `View` 개체를 반환합니다. 이 조각은 뷰를 만들기 위해 레이아웃 파일을 사용하지 않습니다. 대신 인용구를 저장할 **TextView**를 인스턴스화하여 뷰를 프로그래밍 방식으로 만들고 **ScrollView**에 해당 위젯을 표시합니다.

> [!NOTE]
> 조각 서브클래스는 매개 변수가 없는 공용 기본 생성자를 포함해야 합니다.

## <a name="4-create-the-playquoteactivity"></a>4. PlayQuoteActivity 만들기

조각이 작업 내에 호스트되어야 하므로, 이 앱에는 `PlayQuoteFragment`를 호스트하는 작업이 필요합니다. 작업은 런타임에 조각을 해당 레이아웃에 동적으로 추가합니다. 애플리케이션에 새 작업을 추가하고 이름을 `PlayQuoteActivity`로 지정합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![프로젝트에 Android 작업 추가](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![프로젝트에 Android 작업 추가](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

`PlayQuoteActivity`에서 코드 편집:

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

`PlayQuoteActivity`가 생성될 때는 새 `PlayQuoteFragment`를 인스턴스화하고 `FragmentTransaction`의 컨텍스트로 해당 조각을 루트 뷰에 로드합니다. 이 작업은 사용자 인터페이스에 대해 Android 레이아웃 파일을 로드하지 않습니다. 대신 새 `PlayQuoteFragment`가 애플리케이션의 루트 뷰에 추가됩니다. 리소스 식별자 `Android.Resource.Id.Content`는 특정 식별자를 확인하지 않고 작업의 루트 뷰를 참조하기 위해 사용됩니다.

## <a name="5-create-titlesfragment"></a>5. TitlesFragment 만들기

`TitlesFragment`는 조각에서 `ListView`를 표시하기 위한 논리를 캡슐화하는 `ListFragment`로 알려진 특수화된 조각을 서브클래스합니다. `ListFragment`는 `ListAdapter` 속성(`ListView`에서 해당 콘텐츠를 표시하기 위해 사용됨) 및 조각이 `ListView`로 표시되는 행 클릭에 응답할 수 있게 해주는 `OnListItemClick`이라는 이벤트 처리기를 노출합니다.

시작하려면 새 조각을 프로젝트에 추가하고 이름을 **TitlesFragment**로 지정합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![프로젝트에 Android 조각 추가](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![프로젝트에 Android 조각 추가](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

조각 내에서 코드를 편집합니다.

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

작업이 생성되면 Android가 조각의 `OnActivityCreated` 메서드를 호출합니다. 여기에서 `ListView`의 목록 어댑터가 생성됩니다.  `ShowQuoteFromPlay` 메서드가 선택된 희곡의 인용구를 표시하기 위해 `PlayQuoteActivity`의 인스턴스를 시작합니다.

## <a name="display-titlesfragment-in-mainactivity"></a>MainActivity에 TitlesFragment 표시

마지막 단계는 `MainActivity` 내에 `TitlesFragment`를 표시하는 것입니다. 이 작업은 조각을 자동으로 로드하지 않습니다. 대신 `fragment` 요소를 사용해서 작업의 레이아웃 파일에 조각을 선언하여 조각이 정적으로 로드됩니다. 로드할 조각은 `android:name` 특성을 조각 클래스(형식의 네임스페이스 포함)로 설정하여 식별됩니다. 예를 들어 `TitlesFragment`를 사용하려면 `android:name`을 `FragmentSample.TitlesFragment`로 설정합니다.

레이아웃 파일 **activity_main.axml**을 편집하고 기존 XML을 다음과 같이 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> `class` 특성은 `android:name`의 올바른 대체 특성입니다. 선호 양식에 대한 공식적인 지침은 없으며, `android:name`과 서로 교환해서 `class`를 사용하는 코드베이스에 대한 많은 예가 있습니다.

MainActivity에는 필요한 코드 변경이 없습니다. 이 클래스의 코드는 다음 코드 조각과 매우 비슷합니다.

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>앱 실행

이제 코드가 완료되었으므로 디바이스에서 앱을 실행하여 작동하는지 확인합니다.

[![휴대폰에서 실행되는 애플리케이션 스크린샷](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[이 연습의 2부](./walkthrough-landscape.md)에서는 가로 모드로 실행되는 디바이스를 위해 이 애플리케이션을 최적화합니다.
