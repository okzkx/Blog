# 事件系统

将图片制作成自定义按钮

```text
using UnityEngine;
using UnityEngine.EventSystems;

public class PhoneMoveButton : MonoBehaviour {
    EventTrigger EventTrigger;
    bool pressing;
    public bool Pressing => pressing;
    private void Awake() {
        EventTrigger = gameObject.AddComponent<EventTrigger>();
    }

    void Start() {
        {
            var entry = new EventTrigger.Entry();
            entry.eventID = EventTriggerType.PointerDown;
            entry.callback.AddListener(_ => pressing = true);
            EventTrigger.triggers.Add(entry);
        }

        {
            var entry = new EventTrigger.Entry();
            entry.eventID = EventTriggerType.PointerUp;
            entry.callback.AddListener(_ => pressing = false);
            EventTrigger.triggers.Add(entry);
        }

        {
            var entry = new EventTrigger.Entry();
            entry.eventID = EventTriggerType.PointerExit;
            entry.callback.AddListener(_ => pressing = false);
            EventTrigger.triggers.Add(entry);
        }
    }
}
```

