# Migration `20210128152210-first`

This migration has been generated by Tenzin Sean  at 1/28/2021, 4:22:10 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "Post" (
"id" SERIAL,
    "title" TEXT NOT NULL,
    "content" TEXT,
    "published" BOOLEAN NOT NULL DEFAULT false,
    "authorId" INTEGER,

    PRIMARY KEY ("id")
)

CREATE TABLE "User" (
"id" SERIAL,
    "email" TEXT NOT NULL,
    "password" TEXT NOT NULL DEFAULT E'',
    "name" TEXT,

    PRIMARY KEY ("id")
)

CREATE UNIQUE INDEX "User.email_unique" ON "User"("email")

ALTER TABLE "Post" ADD FOREIGN KEY("authorId")REFERENCES "User"("id") ON DELETE SET NULL ON UPDATE CASCADE
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration ..20210128152210-first
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,25 @@
+generator client {
+  provider = "prisma-client-js"
+}
+
+datasource db {
+  provider = "postgresql"
+  url = "***"
+}
+
+model Post {
+  id        Int     @default(autoincrement()) @id
+  title     String
+  content   String?
+  published Boolean @default(false)
+  author    User?   @relation(fields: [authorId], references: [id])
+  authorId  Int?
+}
+
+model User {
+  id       Int     @default(autoincrement()) @id
+  email    String  @unique
+  password String  @default("")
+  name     String?
+  posts    Post[]
+}
```


