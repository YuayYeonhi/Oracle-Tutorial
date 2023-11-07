# DDL and SQL

## DDL

<procedure title="Create Table" id="create_table">
    <p>Create Table: Info</p><img src="createInfo.png" alt="Info"/>
    <code-block lang="sql">
    create table Info(
        id varchar2(10),
        name varchar2(50) not null,
        constraint pk_info primary key(id)
    );
    </code-block>
    <p>Table: Sc</p><img src="tableSc.png" alt="Sc"/>
    <code-block lang="sql">
    create table sc(
        id varchar2(10),
        subject varchar2(30) not null,
        score integer not null,
        constraint uk_sc unique(id,subject),
        constraint fk_sc foreign key(id) references info(id),
        constraint ck_sc_score check(score between 0 and 100)
    );
    </code-block>
</procedure>

1. Data Types
    
    `Integer` `Number` `Date` `Varchar2`.. 
2. Constraints

   `Not Null` `Primary Key` `Foreign Key` `Unique` `Check`

Note: Table name in upper case in query

<procedure title="Describe Table" id="describe_table">
    <code-block lang="sql">
    desc info;
    desc sc;
    </code-block>
</procedure>

<procedure title="Alter Table" id="alter_table">
    <step><p>Add a Column</p>
        <code-block lang="sql">
        alter table info add birthday date;
        alter table info add grade integer default 2020 not null;
        </code-block>
    </step>
    <step>Modify Data Type or Data Size of a Column
        <code-block lang="sql">alter table sc modify subject varchar2(50);</code-block>
    </step>
    <step>Modify Default Value of a Column
    <code-block lang="sql">alter table info modify grade default 2021;</code-block></step>
    <step><p>Modified value only affects newly inserted rows</p>
    <code-block lang="sql">alter table sc drop column birthday</code-block>
    <p>Note: Modified value only affects newly inserted rows</p></step>
    <step>Drop a column<code-block lang="sql">alter table info drop column birthday;</code-block></step>
    <step>Drop a constraint<code-block lang="sql">alter table info drop constraint fk_sc id;</code-block></step>
    <step>Add a Constraint<code-block lang="sql">
    alter table sc modify id not null;
    alter table sc add constraint fk_sc_id 
    foreign key(id) reference info(id);
    </code-block></step>
</procedure>

<procedure title=" Table/Column Comments" id="table_column_comments">
    <step>Table<code-block lang="sql">comment on table info is 'Student Information';</code-block></step>
    <step>Column<code-block lang="sql">comment on column info.id is 'Student ID';</code-block></step>
</procedure>

<procedure title="Truncate table" id="truncate_table">
    <code-block lang="sql">truncate table info;</code-block>
    <p>Removes all rows from a table or specified partitions of a table, without logging 
the individual row deletions. TRUNCATE TABLE is similar to the DELETE statement 
with no WHERE clause; however, TRUNCATE TABLE is faster and uses fewer system 
and transaction log resources.</p>
</procedure>

<procedure title="Drop table" id="drop_table">
    <code-block lang="sql">drop table sc;</code-block>
</procedure>

## SQL

<procedure title=" DQL: Data Query Language" id="dql_data_query_language">
    <code-block lang="sql">select /**/ from /**/ where /**/</code-block>
</procedure>

<procedure title="DML: Data Manipulation Language" id="dml_data_manipulation_language">
    <code-block lang="sql">insert /**/ update /**/  delete /**/ </code-block>
</procedure>

<procedure title="TPL: Transaction Processing Language" id="tpl_transaction_processing_language">
    <code-block lang="sql">commit  rollback  savepoint</code-block>
</procedure>

<procedure title="DCL: Data Control Language" id="dcl_data_control_language">
    <code-block lang="sql">grant revoke</code-block>
</procedure>

<procedure title=" DDL: Data Definition Language" id="ddl_data_definition_language">
    <code-block lang="sql">create alter drop truncate rename</code-block>
</procedure>

<procedure title="CCL: Cursor Control Language" id="ccl_cursor_control_language">
    <code-block lang="sql">cursor</code-block>
</procedure>
