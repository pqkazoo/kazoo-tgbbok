# Limits

* Account limits document..
  * make sure jonny5 is running (use sup ..)
  * Make sure hotornot is running?
* Schema https://github.com/2600hz/kazoo/blob/master/applications/crossbar/priv/couchdb/schemas/limits.json

* https://2600hz.atlassian.net/wiki/display/Dedicated/Account+Limits
* https://2600hz.atlassian.net/wiki/display/Dedicated/Limits
  *  "default_to" email address in system_config/notify.system_alert 
    * example       "default_to": "wlloyd@stormqloud.ca"

```
   "default": {
       "default_text_template": "Alert\n{{message}}\n\nProducer:\n{% for key, value in request %}{{ key }}: {{ value }}\n{% endfor %}\n{% if details %}Details\n{% for key, value in details %}{{ key }}: {{ value }}\n{% endfor %}\n{% endif %}{% if account %}Account\nAccount ID: {{account.pvt_account_id}}\nAccount Name: {{account.name}}\nAccount Realm: {{account.realm}}\n\n{% endif %}{% if admin %}Admin\nName: {{admin.first_name}} {{admin.last_name}}\nEmail: {{admin.email}}\nTimezone: {{admin.timezone}}\n\n{% endif %}{% if account.pvt_wnm_numbers %}Phone Numbers\n{% for number in account.pvt_wnm_numbers %}{{number}}\n{% endfor %}\n{% endif %}Service\nURL: {{service.url}}\nName: {{service.name}}\nService Provider: {{service.provider}}\n\nSent from {{service.host}}",
       "default_html_template": "<html><head><meta charset=\"utf-8\" /></head><body><h2>Alert</h2><p>{{message}}</p><h2>Producer</h2><table cellpadding=\"4\" cellspacing=\"0\" border=\"0\">{% for key, value in request %}<tr><td>{{ key }}: </td><td>{{ value }}</td></tr>{% endfor %}</table>{% if details %}<h2>Details</h2><table cellpadding=\"4\" cellspacing=\"0\" border=\"0\">{% for key, value in details %}<tr><td>{{ key }}: </td><td>{{ value }}</td></tr>{% endfor %}</table>{% endif %}{% if account %}<h2>Account</h2><table cellpadding=\"4\" cellspacing=\"0\" border=\"0\"><tr><td>Account ID: </td><td>{{account.pvt_account_id}}</td></tr><tr><td>Account Name: </td><td>{{account.name}}</td></tr><tr><td>Account Realm: </td><td>{{account.realm}}</td></tr></table>{% endif %}{% if admin %}<h2>Admin</h2><table cellpadding=\"4\" cellspacing=\"0\" border=\"0\"><tr><td>Name: </td><td>{{admin.first_name}} {{admin.last_name}}</td></tr><tr><td>Email: </td><td>{{admin.email}}</td></tr><tr><td>Timezone: </td><td>{{admin.timezone}}</td></tr></table>{% endif %}{% if account.pvt_wnm_numbers %}<h2>Phone Numbers</h2><ul>{% for number in account.pvt_wnm_numbers %}<li>{{number}}</li>{% endfor %}</ul>{% endif %}<h2>Service</h2><table cellpadding=\"4\" cellspacing=\"0\" border=\"0\"><tr><td>URL: </td><td>{{service.url}}</td></tr><tr><td>Name: </td><td>{{service.name}}</td></tr><tr><td>Service Provider: </td><td>{{service.provider}}</td></tr></table><p style=\"font-size:9pt;color:#CCCCCC\">Sent from {{service.host}}</p></body></html>",
       "default_subject_template": "2600hz-Platform: {{request.level}} from {{request.node}}",
       "default_service_url": "http://apps.2600hz.com",
       "default_service_name": "VOIP Services",
       "default_service_provider": "2600hz",
       "default_support_number": "(415) 886-7900",
       "default_support_email": "support@2600hz.com",
       "default_from": "no_reply@k6.prodosec.com",
       "default_template_charset": "",
       "default_to": "wlloyd@prodosec.com"
   },
   "pvt_account_id": "system_config",
   "pvt_account_db": "system_config",
   "pvt_created": 63584408587,
   "pvt_modified": 63584408772,
   "pvt_type": "config"
   }
```
   
  * Set "authz_enabled": 'true' on system_config/ecallmgr

   

Set "authz_enabled" to true on system_config/ecallmgr
Set "authz_default_action" to either "allow" or "deny" on system_config/ecallmgr.  This is used when jonny5 fails to reply.
If you just want to test (and not actually limit) set "authz_dry_run" to true on system_config/ecallmgr.

```
{
   "_id": "limits",
   "_rev": "1-7f33de24a1fe71380b7d7023cf3e23b5",
   "inbound_trunks": 0,
   "twoway_trunks": 1,
   "allow_prepay": true,
   "id": "limits",
   "pvt_type": "limits",
   "pvt_vsn": "1",
```

```

 "inbound_trunks": 0,
   "twoway_trunks": 0,
   "allow_prepay": false,
   "id": "limits",
   "pvt_allow_postpay": true,
   "pvt_allow_prepay": false,
   "pvt_max_postpay_amount": 100,
   "ui_metadata": {
       "ui": "kazoo-ui",
       "version": "RT-2.16(k3.12)(UI1)"
   },
   "pvt_vsn": 1,
   "pvt_modified": 63569634590,
   "pvt_created": 63568813682,
   "pvt_type": "limits",
   "pvt_enabled": true,
```

* No calls on system..
```
[root@k src]# sup jonny5_maintenance authz_summary
no channels found
[root@k src]# sup jonny5_maintenance limits_summary
no limits found
```

* 1 call up
```
[root@k src]# sup jonny5_maintenance limits_summary
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
| Account ID                       | Calls | Resource Calls | Allotments |           Trunks         | Per Minute | Max Postpay |
|                                  |       |                |            |  In | Out | Both | Burst |            |             |
+==================================+=======+================+============+==========================+============+=============+
| 35822499eedbed49facb241a2f12ba5b | -1    | -1             | 0          | 0   | 0   | -1   | 0     | 0.0        | disabled    |
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
| d4ef4d857be41bcaaf6cae02dc50fbd6 | -1    | -1             | 0          | 0   | 0   | -1   | 0     | 111.11     | disabled    |
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
[root@k src]# sup jonny5_maintenance authz_summary
+----------------------------------+-------+----------------+------------+----------------+-----------------+------------+
| Account ID                       | Calls | Resource Calls | Allotments | Inbound Trunks | Outbound Trunks | Per Minute |
+==================================+=======+================+============+================+=================+============+
| d4ef4d857be41bcaaf6cae02dc50fbd6 | 1     | 1              | 0          | 0              | 1               | 0          |
+----------------------------------+-------+----------------+------------+----------------+-----------------+------------+
| 35822499eedbed49facb241a2f12ba5b | 1     | 1              | 0          | 0              | 1               | 0          |
+----------------------------------+-------+----------------+------------+----------------+-----------------+------------+

```

* Call ended, switch quiet again

```

[root@k src]# sup jonny5_maintenance limits_summary
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
| Account ID                       | Calls | Resource Calls | Allotments |           Trunks         | Per Minute | Max Postpay |
|                                  |       |                |            |  In | Out | Both | Burst |            |             |
+==================================+=======+================+============+==========================+============+=============+
| 35822499eedbed49facb241a2f12ba5b | -1    | -1             | 0          | 0   | 0   | -1   | 0     | 0.0        | disabled    |
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
| d4ef4d857be41bcaaf6cae02dc50fbd6 | -1    | -1             | 0          | 0   | 0   | -1   | 0     | 111.11     | disabled    |
+----------------------------------+-------+----------------+------------+--------------------------+------------+-------------+
[root@k src]# sup jonny5_maintenance authz_summary
no channels found


```

```
[root@k src]# sup jonny5_maintenance limits_details 35822499eedbed49facb241a2f12ba5b
Account Info:
  Account ID             : 35822499eedbed49facb241a2f12ba5b
  Account DB             : account%2F35%2F82%2F2499eedbed49facb241a2f12ba5b
  Current Balance        : 0.0
Configuration:
  Enabled                : true
  Prepay Allowed         : true
  Postpay Allowed        : false
  Max Postpay Amount     : 0.0
  Reserve Amount         : 0.5
  Soft Limit Inbound     : false
  Soft Limit Outbound    : false
Limits:
  Calls Hard Limit       : -1
  Resources Hard Limit   : -1
  Inbound Trunks         : 0
  Bundled Inbound Trunks : 0
  Outbound Trunks        : 0
  Bundled Outbound Trunks: 0
  Twoway Trunks          : -1
  Bundled Twoway Trunks  : 0
Allotments:
  -none-
```
