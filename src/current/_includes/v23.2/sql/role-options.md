Role option | Description
------------|-------------
`CANCELQUERY`/`NOCANCELQUERY` | **Deprecated in v22.2: Use the `CANCELQUERY` [system privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to cancel [queries]({% link {{ page.version.version }}/cancel-query.md %}) and [sessions]({% link {{ page.version.version }}/cancel-session.md %}) of other roles. Without this role option, roles can only cancel their own queries and sessions. Even with the `CANCELQUERY` role option, non-`admin` roles cannot cancel `admin` queries or sessions. This option should usually be combined with `VIEWACTIVITY` so that the role can view other roles' query and session information. <br><br>By default, the role option is set to `NOCANCELQUERY` for all non-`admin` roles.
`CONTROLCHANGEFEED`/`NOCONTROLCHANGEFEED` | **Deprecated in v23.1: Use the `CHANGEFEED` [privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to run [`CREATE CHANGEFEED`]({% link {{ page.version.version }}/create-changefeed.md %}) on tables they have `SELECT` privileges on. <br><br>By default, the role option is set to `NOCONTROLCHANGEFEED` for all non-`admin` roles.
`CONTROLJOB`/`NOCONTROLJOB` | Allow or disallow a role to [pause]({% link {{ page.version.version }}/pause-job.md %}), [resume]({% link {{ page.version.version }}/resume-job.md %}), and [cancel]({% link {{ page.version.version }}/cancel-job.md %}) jobs. Non-`admin` roles cannot control jobs created by `admin` roles. <br><br>By default, the role option is set to `NOCONTROLJOB` for all non-`admin` roles.
`CREATEDB`/`NOCREATEDB` | Allow or disallow a role to [create]({% link {{ page.version.version }}/create-database.md %}) or [rename]({% link {{ page.version.version }}/alter-database.md %}#rename-to) a database. The role is assigned as the owner of the database. <br><br>By default, the role option is set to `NOCREATEDB` for all non-`admin` roles.
`CREATELOGIN`/`NOCREATELOGIN` | Allow or disallow a role to manage authentication using the `WITH PASSWORD`, `VALID UNTIL`, and `LOGIN/NOLOGIN` role options. <br><br>By default, the role option is set to `NOCREATELOGIN` for all non-`admin` roles.
`CREATEROLE`/`NOCREATEROLE` |  Allow or disallow the new role to [create]({% link {{ page.version.version }}/create-role.md %}), alter, and [drop]({% link {{ page.version.version }}/drop-role.md %}) other non-`admin` roles. <br><br>By default, the role option is set to `NOCREATEROLE` for all non-`admin` roles.
`LOGIN`/`NOLOGIN` | Allow or disallow a role to log in with one of the [client authentication methods]({% link {{ page.version.version }}/authentication.md %}#client-authentication). Setting the role option to `NOLOGIN` prevents the role from logging in using any authentication method.
`MODIFYCLUSTERSETTING`/`NOMODIFYCLUSTERSETTING` | Allow or disallow a role to modify the [cluster settings]({% link {{ page.version.version }}/cluster-settings.md %}) with the `sql.defaults` prefix. <br><br>By default, the role option is set to `NOMODIFYCLUSTERSETTING` for all non-`admin` roles.
`PASSWORD password`/`PASSWORD NULL` | The credential the role uses to [authenticate their access to a secure cluster]({% link {{ page.version.version }}/authentication.md %}#client-authentication). A password should be entered as a [string literal]({% link {{ page.version.version }}/sql-constants.md %}#string-literals). For compatibility with PostgreSQL, a password can also be entered as an identifier. <br><br>To prevent a role from using [password authentication]({% link {{ page.version.version }}/authentication.md %}#client-authentication) and to mandate [certificate-based client authentication]({% link {{ page.version.version }}/authentication.md %}#client-authentication), [set the password as `NULL`]({% link {{ page.version.version }}/create-role.md %}#prevent-a-role-from-using-password-authentication).
`SQLLOGIN`/`NOSQLLOGIN` | **Deprecated in v22.2: Use the `NOSQLLOGIN` [system privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to log in using the SQL CLI with one of the [client authentication methods]({% link {{ page.version.version }}/authentication.md %}#client-authentication). The role option to `NOSQLLOGIN` prevents the role from logging in using the SQL CLI with any authentication method while retaining the ability to log in to DB Console. It is possible to have both `NOSQLLOGIN` and `LOGIN` set for a role and `NOSQLLOGIN` takes precedence on restrictions. <br><br>Without any role options all login behavior is permitted.
`VALID UNTIL` |  The date and time (in the [`timestamp`]({% link {{ page.version.version }}/timestamp.md %}) format) after which the [password](#parameters) is not valid.
`VIEWACTIVITY`/`NOVIEWACTIVITY` | **Deprecated in v22.2: Use the `VIEWACTIVITY` [system privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to see other roles' [queries]({% link {{ page.version.version }}/show-statements.md %}) and [sessions]({% link {{ page.version.version }}/show-sessions.md %}) using `SHOW STATEMENTS`, `SHOW SESSIONS`, and the [**Statements**](ui-statements-page.html) and [**Transactions**](ui-transactions-page.html) pages in the DB Console. `VIEWACTIVITY` also permits visibility of node hostnames and IP addresses in the DB Console. With `NOVIEWACTIVITY`, the `SHOW` commands show only the role's own data, and DB Console pages redact node hostnames and IP addresses.<br><br>By default, the role option is set to `NOVIEWACTIVITY` for all non-`admin` roles.
`VIEWCLUSTERSETTING` / `NOVIEWCLUSTERSETTING` | **Deprecated in v22.2: Use the `VIEWCLUSTERSETTING` [system privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to view the [cluster settings]({% link {{ page.version.version }}/cluster-settings.md %}) with `SHOW CLUSTER SETTING` or to access the [**Cluster Settings**]({% link {{ page.version.version }}/ui-debug-pages.md %}) page in the DB Console. <br><br>By default, the role option is set to `NOVIEWCLUSTERSETTING` for all non-`admin` roles.
`VIEWACTIVITYREDACTED`/`NOVIEWACTIVITYREDACTED` | **Deprecated in v22.2: Use the `VIEWACTIVITYREDACTED` [system privilege]({% link {{ page.version.version }}/security-reference/authorization.md %}#supported-privileges).** Allow or disallow a role to see other roles' queries and sessions using `SHOW STATEMENTS`, `SHOW SESSIONS`, and the Statements and Transactions pages in the DB Console. With `VIEWACTIVITYREDACTED`, a user will not have access to the usage of statements diagnostics bundle (which can contain PII information) in the DB Console, and will not be able to list queries containing [constants]({% link {{ page.version.version }}/sql-constants.md %}) for other users when using the `listSessions` endpoint through the [Cluster API]({% link {{ page.version.version }}/cluster-api.md %}). It is possible to have both `VIEWACTIVITY` and `VIEWACTIVITYREDACTED`, and `VIEWACTIVITYREDACTED` takes precedence on restrictions. If the user has `VIEWACTIVITY` but doesn't have `VIEWACTIVITYREDACTED`, they will be able to see DB Console pages and have access to the statements diagnostics bundle. <br><br>By default, the role option is set to `NOVIEWACTIVITYREDACTED` for all non-`admin` roles.