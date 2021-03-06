# Migration `20201209075842-add-other-items`

This migration has been generated by Devinda Senanayaka at 12/9/2020, 1:58:42 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "Finance" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "accountBalance" REAL NOT NULL
)

CREATE TABLE "Announcement" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "title" TEXT NOT NULL,
    "detail" TEXT NOT NULL,
    "imageUrl" TEXT NOT NULL
)

CREATE TABLE "ActionItem" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "title" TEXT NOT NULL,
    "detail" TEXT NOT NULL,
    "completed" BOOLEAN NOT NULL,
    "studentId" INTEGER,

    FOREIGN KEY ("studentId") REFERENCES "Student"("id") ON DELETE SET NULL ON UPDATE CASCADE
)

PRAGMA foreign_keys=OFF;
CREATE TABLE "new_Student" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "profilePicURL" TEXT NOT NULL,
    "creditsCompleted" INTEGER NOT NULL,
    "studentID" INTEGER NOT NULL,
    "gender" TEXT NOT NULL,
    "dob" TEXT NOT NULL,
    "birthCountry" TEXT NOT NULL,
    "birthState" TEXT NOT NULL,
    "maritialStatus" TEXT NOT NULL,
    "militaryStatus" TEXT NOT NULL,
    "citizenshipStatus" TEXT NOT NULL,
    "citizenshipCountry" TEXT NOT NULL,
    "visaType" TEXT NOT NULL,
    "visaPermit" TEXT NOT NULL,
    "visaCountry" TEXT NOT NULL,
    "financeId" INTEGER NOT NULL,
    "advisorId" INTEGER,

    FOREIGN KEY ("financeId") REFERENCES "Finance"("id") ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY ("advisorId") REFERENCES "Teacher"("id") ON DELETE SET NULL ON UPDATE CASCADE
);
INSERT INTO "new_Student" ("id", "studentID", "gender", "dob", "birthCountry", "birthState", "maritialStatus", "militaryStatus", "citizenshipStatus", "citizenshipCountry", "visaType", "visaPermit", "visaCountry", "advisorId") SELECT "id", "studentID", "gender", "dob", "birthCountry", "birthState", "maritialStatus", "militaryStatus", "citizenshipStatus", "citizenshipCountry", "visaType", "visaPermit", "visaCountry", "advisorId" FROM "Student";
DROP TABLE "Student";
ALTER TABLE "new_Student" RENAME TO "Student";
PRAGMA foreign_key_check;
PRAGMA foreign_keys=ON
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20201209061052-address-tableaddresses-toaddress..20201209075842-add-other-items
--- datamodel.dml
+++ datamodel.dml
@@ -3,13 +3,15 @@
 }
 datasource db {
   provider = "sqlite"
-  url = "***"
+  url = "***"
 }
 model Student {
   id                      Int     @id @default(autoincrement())
+  profilePicURL           String
+  creditsCompleted        Int
   studentID               Int
   gender                  String
   dob                     String
   birthCountry            String
@@ -22,12 +24,14 @@
   visaPermit              String
   visaCountry             String
   names                   Name[]
   addresses               Address[]
-  phonenumbers             PhoneNumber[]
+  phonenumbers            PhoneNumber[]
   emails                  EmailAddress[]
   contacts                Contact[]
-
+  finance                 Finance @relation(fields: [financeId], references: [id])
+  financeId               Int 
+  actionItems             ActionItem[]
   advisor   Teacher? @relation(fields: [advisorId], references: [id])
   advisorId Int?
   classes   Class[] @relation("RegistrationRecord")
 }
@@ -55,8 +59,26 @@
   type      String
   address   String
   preferred Boolean
 }
+model Finance {
+    id              Int @id @default(autoincrement())
+    accountBalance  Float
+}
+model Announcement {
+    id        Int @id @default(autoincrement())
+    title     String
+    detail    String
+    imageUrl  String
+}
+
+model ActionItem {
+    id        Int @id @default(autoincrement())
+    title     String
+    detail    String
+    completed Boolean
+}
+
 model Contact{
     id              Int @id @default(autoincrement())
     primary         Boolean
     name            String
```


