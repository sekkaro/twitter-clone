# Migration `20210320165130-post-to-tweet`

This migration has been generated at 3/21/2021, 1:51:30 AM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."Post" DROP CONSTRAINT "Post_authorId_fkey"

DROP TABLE "public"."Post"
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20210320164613-profile..20210320165130-post-to-tweet
--- datamodel.dml
+++ datamodel.dml
@@ -3,28 +3,26 @@
 }
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url = "***"
 }
-model Post {
-  id        Int     @id @default(autoincrement())
-  title     String
-  content   String?
-  published Boolean @default(false)
-  author    User?   @relation(fields: [authorId], references: [id])
+model Tweet {
+  id        Int      @id @default(autoincrement())
+  createdAt DateTime @default(now())
+  content   String
+  author    User?    @relation(fields: [authorId], references: [id])
   authorId  Int?
 }
 model User {
   id       Int      @id @default(autoincrement())
   email    String   @unique
   password String   @default("")
   name     String?
-  posts    Post[]
+  tweets   Tweet[]
   Profile  Profile?
-  Tweet    Tweet[]
 }
 model Profile {
   id        Int      @id @default(autoincrement())
@@ -35,12 +33,4 @@
   avatar    String?
   userId    Int?     @unique
   User      User?    @relation(fields: [userId], references: [id])
 }
-
-model Tweet {
-  id        Int      @id @default(autoincrement())
-  createdAt DateTime @default(now())
-  content   String
-  author    User?    @relation(fields: [authorId], references: [id])
-  authorId  Int?
-}
```


