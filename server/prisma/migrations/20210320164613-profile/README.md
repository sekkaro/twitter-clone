# Migration `20210320164613-profile`

This migration has been generated at 3/21/2021, 1:46:13 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "public"."Profile" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"bio" text   ,
"location" text   ,
"website" text   ,
"avatar" text   ,
"userId" integer   ,
PRIMARY KEY ("id")
)

CREATE TABLE "public"."Tweet" (
"id" SERIAL,
"createdAt" timestamp(3)   NOT NULL DEFAULT CURRENT_TIMESTAMP,
"content" text   NOT NULL ,
"authorId" integer   ,
PRIMARY KEY ("id")
)

CREATE UNIQUE INDEX "Profile.userId_unique" ON "public"."Profile"("userId")

ALTER TABLE "public"."Profile" ADD FOREIGN KEY("userId")REFERENCES "public"."User"("id") ON DELETE SET NULL ON UPDATE CASCADE

ALTER TABLE "public"."Tweet" ADD FOREIGN KEY("authorId")REFERENCES "public"."User"("id") ON DELETE SET NULL ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20210317132958-first..20210320164613-profile
--- datamodel.dml
+++ datamodel.dml
@@ -3,23 +3,44 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url = "***"
 }
 model Post {
-  id        Int     @default(autoincrement()) @id
+  id        Int     @id @default(autoincrement())
   title     String
   content   String?
   published Boolean @default(false)
   author    User?   @relation(fields: [authorId], references: [id])
   authorId  Int?
 }
 model User {
-  id       Int     @default(autoincrement()) @id
-  email    String  @unique
-  password String  @default("")
+  id       Int      @id @default(autoincrement())
+  email    String   @unique
+  password String   @default("")
   name     String?
   posts    Post[]
-}
+  Profile  Profile?
+  Tweet    Tweet[]
+}
+
+model Profile {
+  id        Int      @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  bio       String?
+  location  String?
+  website   String?
+  avatar    String?
+  userId    Int?     @unique
+  User      User?    @relation(fields: [userId], references: [id])
+}
+
+model Tweet {
+  id        Int      @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  content   String
+  author    User?    @relation(fields: [authorId], references: [id])
+  authorId  Int?
+}
```


