---
description: Useful SQL Templates
---
# SQL Templates

Some SQL editors allow you to create reusable templates with auto-completion. For example on DBeaver you can do this by clicking on click Configure [⚙️] in the bottom toolbar, then click Preferences -> SQL Editor -> Templates. [Read more here](https://github.com/dbeaver/dbeaver/wiki/SQL-Templates)

## Creating tables

ID upgrade? PG>=10 https://www.2ndquadrant.com/en/blog/postgresql-10-identity-columns/

```sql
create sequence "public"."${cursor}_id_seq";

create table "public"."${cursor}" (
    "id" bigint not null default nextval('${cursor}_id_seq'::regclass),
    "created_by" bigint, -- store the user who created the record
    "updated_by" bigint, -- store the user who last updated the record
    "inserted_at" timestamp without time zone not null default now(),
    "updated_at" timestamp without time zone not null default now()
);


CREATE UNIQUE INDEX ${cursor}_pkey ON public.${cursor} USING btree (id);

alter table "public"."${cursor}" add constraint "${cursor}_pkey" PRIMARY KEY using index "${cursor}_pkey";
alter table "public"."${cursor}" add constraint "${cursor}_created_by_fkey" FOREIGN KEY (created_by) REFERENCES users(id);
alter table "public"."${cursor}" add constraint "${cursor}_updated_by_fkey" FOREIGN KEY (updated_by) REFERENCES users(id);
alter table "public"."${cursor}" add constraint "${cursor}_user_id_fkey" FOREIGN KEY (user_id) REFERENCES users(id);

```