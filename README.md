# 📜 Salesforce Apex Trigger Framework - Kevin O'Hara Approach

## 📖 Introduction
The Kevin O'Hara Trigger Framework is a widely adopted design pattern in Salesforce development that ensures:
- Only **one trigger per object**.
- All business logic resides outside the trigger in a dedicated **Trigger Handler class**.
- Easy maintenance, extension, and testing.
- Separation of Concerns (Trigger = Entry Point, Handler = Business Logic).

---

## 📂 Recommended Folder Structure

src/ |-- triggers/ | |-- AccountTrigger.trigger |-- classes/ | |-- TriggerHandler.cls | |-- AccountTriggerHandler.cls README.md


---

## 🚀 Code Implementation

### 1️⃣ Account Trigger (AccountTrigger.trigger)

```apex
trigger AccountTrigger on Account (
    before insert, before update, before delete,
    after insert, after update, after delete, after undelete
) {
    AccountTriggerHandler handler = new AccountTriggerHandler();
    handler.run(Trigger.operationType, Trigger.new, Trigger.oldMap);
}

public abstract class TriggerHandler {

    public void run(System.TriggerOperation triggerOperation, List<SObject> newRecords, Map<Id, SObject> oldRecordsMap) {
        switch on triggerOperation {
            when BEFORE_INSERT {
                beforeInsert(newRecords);
            }
            when BEFORE_UPDATE {
                beforeUpdate(newRecords, oldRecordsMap);
            }
            when BEFORE_DELETE {
                beforeDelete(oldRecordsMap);
            }
            when AFTER_INSERT {
                afterInsert(newRecords);
            }
            when AFTER_UPDATE {
                afterUpdate(newRecords, oldRecordsMap);
            }
            when AFTER_DELETE {
                afterDelete(oldRecordsMap);
            }
            when AFTER_UNDELETE {
                afterUndelete(newRecords);
            }
        }
    }

    protected virtual void beforeInsert(List<SObject> newRecords) { }
    protected virtual void beforeUpdate(List<SObject> newRecords, Map<Id, SObject> oldRecordsMap) { }
    protected virtual void beforeDelete(Map<Id, SObject> oldRecordsMap) { }
    protected virtual void afterInsert(List<SObject> newRecords) { }
    protected virtual void afterUpdate(List<SObject> newRecords, Map<Id, SObject> oldRecordsMap) { }
    protected virtual void afterDelete(Map<Id, SObject> oldRecordsMap) { }
    protected virtual void afterUndelete(List<SObject> newRecords) { }
}

src/
|-- triggers/
|   |-- AccountTrigger.trigger
|-- classes/
|   |-- TriggerHandler.cls
|   |-- AccountTriggerHandler.cls
README.md (This file - explaining the framework)
