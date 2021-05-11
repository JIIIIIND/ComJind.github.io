---
title: "인턴 1주차 후기"
excerpt: "SISOUL 인턴 1주차"
date: 2021-05-10 19:35:22 +0900
categories: 42Seoul
---

안드로이드 개발자로 (주)시솔지주에 한달 간 인턴 생활을 시작했습니다.
사실 안드로이드 개발을 본격적으로 시작한지 얼마 되지도 않았고, 기본이 제대로 다져지지도 않았기에 많이 걱정을 하고 있었는데, 크게 어려운 일은 없을거라는 말에 우선은 좋은 경험이라고 생각했습니다.

실력적으로 검증이 되지 않았기 때문에 우선은 간단한 과제 몇 가지를 해결하게 되었습니다.
첫 주의 과제는 5일의 기간동안 NXP의 TagInfo 어플의 기능과 시솔지주의 지문 인식 카드가 동작할 수 있는 어플을 구현하는 것이었습니다.

샘플 코드는 건네받았지만, 각 데이터들이 어떤 정보를 의미하는지 몰랐기에 NFC의 각 태그 종류와 NFC를 통한 메시지 전송 등에 대한 이론적인 부분을 살펴봤습니다.
다음으론 안드로이드에서 NFC 태그를 인식할 수 있도록 구현을 했습니다.

```kotlin
class NFCHandler(activity: Activity) {
    private val TAG = "NFC_HANDLER"
    private var activity : Activity? = activity
    private var nfcAdapter : NfcAdapter? = null
    private var tag : Tag? = null

    init {
        nfcAdapter = NfcAdapter.getDefaultAdapter(this.activity)
        if (this.nfcAdapter == null)
            Toast.makeText(activity, "NFC를 지원하지 않습니다.", Toast.LENGTH_SHORT).show()
    }

    fun activateNfcController() : Unit {
        if (this.nfcAdapter != null && this.activity != null) {
            val targetIntent = Intent(this.activity, this.activity!!::class.java)
            targetIntent.flags = Intent.FLAG_ACTIVITY_SINGLE_TOP
            //pending
            val pendingIntent = PendingIntent.getActivities(this.activity, 0, arrayOf(targetIntent), 0)
            //intentFilter
            val intentFilter : Array<IntentFilter>? = arrayOf(IntentFilter(NfcAdapter.ACTION_NDEF_DISCOVERED))
            val techLists = arrayOf(
                arrayOf(NfcA::class.java.name),
                arrayOf(NfcF::class.java.name),
                arrayOf(NfcV::class.java.name),
                arrayOf(NfcB::class.java.name))
            //techLists
            nfcAdapter!!.enableForegroundDispatch(this.activity, pendingIntent, intentFilter, techLists)
        }
    }

    fun deActivateNfcAdapter() {
        if (this.nfcAdapter != null)
            this.nfcAdapter!!.disableForegroundDispatch(this.activity)
    }

    fun getTag(intent : Intent) : Tag? {
        this.tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG)
        if (this.tag == null) {
            Toast.makeText(this.activity, "NFC Tag 실패", Toast.LENGTH_SHORT).show()
            return null
        }
        Toast.makeText(this.activity, "NFC Tag 성공", Toast.LENGTH_SHORT).show()
        return this.tag
    }
	...
}
```

NFCHandler에서 activateNfcController 메서드를 통해 어떤 종류의 NFC를 감지할 것인지 정해주고, foreground에서 동작하도록 합니다.
예를 들어 techLists에서 NfcV를 제외한다면 해당 타입의 NFC태그는 기기가 인식하지 못합니다.

첫날은 여기까지 구현을 했고, 그 이후부터 각 태그별로 읽을 수 있는 데이터를 처리하는 작업, 지문 인식 카드의 동작 순서를 이해하고 코틀린으로 변경하고 UI를 적용하는 작업을 했습니다.
생각보다 기본 틀을 구성하는데 많은 시간이 걸렸고, 3일째에 기능은 모두 구현이 되었지만, UI가 제대로 구현이 되지 않은 상황이라 예시를 들었던 NXP TagInfo의 어플과 유사하게 구현했습니다.

전체적으로 디자인을 하는데 생각보다 많은 시간이 걸렸고, 5일의 기간 내에 메뉴얼도 작성을 해야 했다는 것을 금요일에 구현을 완료한 이후 알게 되었습니다. 결국 주어진 5일의 기간 동안 프로젝트를 완료하지 못한게 되었고, 조금 늦게 어플을 완성하여 출시하고 메뉴얼을 만들었습니다.

안드로이드 개발 환경 자체가 낯설었던 것도 있었지만, 그보다는 시간이 조금 걸리지만 확실히 알고 있는 구조로 만들어서 구현하는 것과 당장 구현할 수 있지만 추후에 확장을 하려면 내부를 수정해야 되는 구조에서 고민을 했던게 시간이 좀 더 걸리게 된 원인이라고 생각됩니다. 좋은 구조로 설계하는 것도 물론 중요하지만 우선은 일정에 맞게 구현을 하고 기능의 확장이 필요해질 때 수정을 하는게 일정 관리에 더 도움이 되지 않았을까 느꼈습니다.