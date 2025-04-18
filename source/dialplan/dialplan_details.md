# Dialplan Details

## Global

Global specific dialplans are global to all tennants(domains). These can be changed, however the changes apply to all tennants.

### Not Found

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | | | | | 0 | 5 |
| action | set | call_direction=inbound | | TRUE | 0 | 10 |
| action | log | [inbound routes] 404 not found \${sip_network_ip} | | TRUE | 0 | 15 |

### Call Forward All

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | \${user_exists} | TRUE | | | 0 | 5 |
| condition | \${forward_all_enabled} | TRUE | | | 0 | 10 |
| action | transfer | \${forward_all_destination} XML \${domain_name} | | | 0 | 15 |

### Intercept Ext Polycom

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\\*97(\\d+)$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | lua | intercept.lua $1 | | | 0 | 15 |

### Talking Clock Date

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\\*9171$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | sleep | 1000 | | | 0 | 15 |
| action | say | \${default_language} CURRENT_DATE pronounced \${strepoch()} | | | 0 | 20 |
| action | hangup | | | | 0 | 25 |

### Talking Clock Date And Time

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\\*9172$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | sleep | 1000 | | | 0 | 15 |
| action | say | \${default_language} CURRENT_DATE_TIME pronounced \${strepoch()} | | | 0 | 20 |
| action | hangup | | | | 0 | 25 |

### Outbound Route Example

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | \${user_exists} | FALSE | | | 0 | 0 |
| condition | destination_number | ^\+?1?(\d{10})$ | | | 0 | 10 |
| action | set | sip_h_X-accountcode=\${accountcode} | | | 0 | 20 |
| action | export | call_direction=outbound | | | 0 | 30 |
| action | unset | call_timeout | | | 0 | 40 |
| action | set | hangup_after_bridge=true | | | 0 | 50 |
| action | set | effective_caller_id_name=\${outbound_caller_id_name} | | | 0 | 60 |
| action | set | effective_caller_id_number=\${outbound_caller_id_number} | | | 0 | 70 |
| action | set | inherit_codec=true | | | 0 | 80 |
| action | set | ignore_display_updates=true | | | 0 | 90 |
| action | set | callee_id_number=$1 | | | 0 | 100 |
| action | set | continue_on_fail=true | | | 0 | 110 |
| action | bridge | sofia/gateway/72d236fb-945b-4c86-8e75-af7c6bcf2862/$1 | | | 0 | 120 |
| action | bridge | sofia/gateway/72d236fb-945b-4c86-8e75-af7c6bcf2862/$1 | | | 0 | 130 |

### Talking Clock Time

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*9170$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | sleep | 1000 | | | 0 | 15 |
| action | say | \${default_language} CURRENT_TIME pronounced \${strepoch()} | | | 0 | 20 |
| action | hangup | | | | 0 | 25 |

***

## Domain Specific

Domain specific dialplans are all the same initially but can be changed. Those changes are per domain, thus helps FusionPBX achieve multitenancy.

### Hold Music

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                              | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|---------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*9664$                                          |                       |                        | 0                     | 5                     |
| condition           | \${sip_has_crypto}    | ^(AES_CM_128_HMAC_SHA1_32\|AES_CM_128_HMAC_SHA1_80)$ |                    |                        | 0                     | 10                    |
| action              | answer               |                                                   |                       |                        | 0                     | 15                    |
| action              | execute_extension    | is_secure XML \${context}                          |                       |                        | 0                     | 20                    |
| action              | playback             | $\${hold_music}                                    |                       |                        | 0                     | 25                    |
| anti-action         | set                  | zrtp_secure_media=true                            |                       |                        | 0                     | 30                    |
| anti-action         | answer               |                                                   |                       |                        | 0                     | 35                    |
| anti-action         | playback             | silence_stream://2000                             |                       |                        | 0                     | 40                    |
| anti-action         | execute_extension    | is_zrtp_secure XML \${context}                     |                       |                        | 0                     | 45                    |
| anti-action         | playback             | $\${hold_music}                                    |                       |                        | 0                     | 50                    |

### Agent Status

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|---------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*22$                    |                       |                        | 0                     | 5                     |
| action              | set                  | agent_id=\${sip_from_user} |                       |                        | 0                     | 10                    |
| action              | lua                  | app.lua agent_status      |                       |                        | 0                     | 15                    |
Conversion Notes:

### Agent Status ID

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\\*23$ | | | 0 | 5 |
| action | set | agent_id= | | | 0 | 10 |
| action | lua | app.lua agent_status | | | 0 | 15 |

### DISA

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*(3472)$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | set | pin_number=36227215 | | | 0 | 15 |
| action | set | dialplan_context=\${context} | | | 0 | 20 |
| action | lua | disa.lua | | | 0 | 25 |

### Provision

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*11$ | on-true | | 0 | 5 |
| action | set | reboot=true | | | 0 | 10 |
| action | set | action=login | | | 0 | 15 |
| action | lua | app.lua provision | | | 0 | 20 |
| condition | destination_number | ^\*12$ | | | 1 | 30 |
| action | set | reboot=true | | | 1 | 35 |
| action | set | action=logout | | | 1 | 40 |
| action | lua | app.lua provision | | | 1 | 45 |

### Call Forward

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*72$ | on-true | | 0 | 5 |
| action | set | request_id=false | | | 0 | 10 |
| action | set | enabled=true | | | 0 | 15 |
| action | lua | call_forward.lua | | | 0 | 20 |
| condition | destination_number | ^\*73$ | on-true | | 1 | 30 |
| action | set | request_id=false | | | 1 | 35 |
| action | set | enabled=false | | | 1 | 40 |
| action | lua | call_forward.lua | | | 1 | 45 |
| condition | destination_number | ^\*74$ | on-true | | 2 | 55 |
| action | set | request_id=false | | | 2 | 60 |
| action | set | enabled=toggle | | | 2 | 65 |
| action | lua | call_forward.lua | | | 2 | 70 |
| condition | destination_number | ^forward\+(\Q\${caller_id_number}\E)(?:\/(\d+))?$ | on-true | | 3 | 80 |
| action | set | enabled=toggle | | | 3 | 85 |
| action | set | forward_all_destination=$2 | | | 3 | 90 |
| action | lua | call_forward.lua | | | 3 | 95 |

### Call Block

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | \${call_direction} | ^inbound$ | | | 0 | 5 |
| action | lua | app.lua call_block | | | 0 | 10 |

### Do Not Disturb

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*77$ | on-true | | 0 | 5 |
| action | set | enabled=toggle | | | 0 | 10 |
| action | lua | do_not_disturb.lua | | | 0 | 15 |
| condition | destination_number | ^\*78$|\*363$ | on-true | | 1 | 25 |
| action | set | enabled=true | | | 1 | 30 |
| action | lua | do_not_disturb.lua | | | 1 | 35 |
| condition | destination_number | ^\*79$ | on-true | | 2 | 45 |
| action | set | enabled=false | | | 2 | 50 |
| action | lua | do_not_disturb.lua | | | 2 | 55 |
| condition | destination_number | ^dnd\+\${caller_id_number}$ | on-true | | 3 | 65 |
| action | set | enabled=toggle | | | 3 | 70 |
| action | lua | do_not_disturb.lua | | | 3 | 75 |

### Voicemail (Vmain User)

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*97$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | sleep | 1000 | | | 0 | 15 |
| action | set | voicemail_action=check | | | 0 | 20 |
| action | set | voicemail_id=\${caller_id_number} | | | 0 | 25 |
| action | set | voicemail_profile=default | | | 0 | 30 |
| action | lua | app.lua voicemail | | | 0 | 35 |

### Vmain

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | \^vmain\$\|\^\*4000\$\|\^\*98\$ | never | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | sleep | 1000 | | | 0 | 15 |
| action | set | voicemail_action=check | | | 0 | 20 |
| action | set | voicemail_profile=default | | | 0 | 25 |
| action | lua | app.lua voicemail | | | 0 | 30 |
| condition | destination_number | \^\(vmain\$\\\|\^\*4000\$\\\|\^\*98\)\(\\d\{2,12\}\)\$ | | | 1 | 40 |
| action | answer | | | | 1 | 45 |
| action | sleep | 1000 | | | 1 | 50 |
| action | set | voicemail_action=check | | | 1 | 55 |
| action | set | voicemail_id=$2 | | | 1 | 60 |
| action | set | voicemail_profile=default | | | 1 | 65 |
| action | set | voicemail_authorized=false | | | 1 | 70 |
| action | lua | app.lua voicemail | | | 1 | 75 |

### Directory

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*411$ | | | 0 | 5 |
| action | lua | directory.lua | | | 0 | 10 |


### Follow Me

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*21$               |                       |                        | 0                     | 5                     |
| action              | answer               |                      |                       |                        | 0                     | 10                    |
| action              | lua                  | follow_me.lua        |                       |                        | 0                     | 15                    |

### Recordings

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*(732)$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | set | pin_number=37775310 | | | 0 | 15 |
| action | set | recording_slots=true | | | 0 | 20 |
| action | set | recording_prefix=recording | | | 0 | 25 |
| action | lua | recordings.lua | | | 0 | 30 |

### Call Privacy

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*67(\d+)$ | | | 0 | 5 |
| action | privacy | full | | | 0 | 10 |
| action | set | sip_h_Privacy=id | | | 0 | 15 |
| action | set | privacy=yes | | | 0 | 20 |
| action | transfer | $1 XML \${context} | | | 0 | 25 |

### Page

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*724$ | | | 0 | 5 |
| action | set | caller_id_name=Page | | | 0 | 10 |
| action | set | caller_id_number= | | | 0 | 15 |
| action | set | pin_number=48760243 | | | 0 | 20 |
| action | set | destinations=101-103,105 | | | 0 | 25 |
| action | set | moderator=false | | | 0 | 30 |
| action | set | mute=true | | | 0 | 35 |
| action | set | set api_hangup_hook=conference page-\${destination_number} kick all | | | 0 | 40 |
| action | lua | page.lua | | | 0 | 45 |

### Valet Park In

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^(park\+)?(\*5900)$ | | | 0 | 5 |
| action | valet_park | park@\${domain_name} auto in 5901 5999 | | | 0 | 10 |

### Valet Park Out

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^(park\+)?\*(59[0-9][0-9])$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | valet_park | park@\${domain_name} $2 | | | 0 | 15 |

### Valet Parking

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^(park\+)?(\*59[0-9][0-9])$ | never | | 0 | 5 |
| condition | \${sip_h_Referred-By} | sip:(.*)@.* | never | | 0 | 10 |
| action | set | referred_by_user=$1 | | | 0 | 15 |
| condition | destination_number | ^(park\+)?(\*59[0-9][0-9])$ | never | | 1 | 25 |
| action | set | park_in_use=false | | TRUE | 1 | 30 |
| action | set | park_lot=$2 | | TRUE | 1 | 35 |
| condition | destination_number | ^(park\+)?(\*59[0-9][0-9])$ | | | 2 | 45 |
| condition | \${cond \${sip_h_Referred-By} == '' ? false : true} | TRUE | never | | 2 | 50 |
| action | set | park_in_use=\${regex \${valet_info park@\${domain_name}}\|\${park_lot}} | | TRUE | 2 | 55 |
| condition | \${park_in_use} | TRUE | never | | 3 | 65 |
| action | transfer | \${referred_by_user} XML \${context} | | | 3 | 70 |
| anti-action | set | valet_parking_timeout=180 | | | 3 | 75 |
| anti-action | set | valet_hold_music=\${hold_music} | | | 3 | 80 |
| anti-action | set | valet_parking_orbit_exten=\${referred_by_user} | | | 3 | 85 |
| anti-action | valet_park | park@\${domain_name} \${park_lot} | | | 3 | 90 |


### User Exists

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                                      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|-----------------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           |                      |                                                           |                       |                        | 0                     | 5                     |
| action              | set                  | user_exists=\${user_exists id \${destination_number} \${domain_name}} | | TRUE           | 0                     | 10                    |
| condition           | \${user_exists}       | ^true$                                                    |                       |                        | 1                     | 20                    |
| action              | set                  | extension_uuid=\${user_data \${destination_number}@\${domain_name} var extension_uuid} | | TRUE  | 1                     | 25                    |
| action              | set                  | hold_music=\${user_data \${destination_number}@\${domain_name} var hold_music} | | TRUE  | 1                     | 30                    |
| action              | set                  | forward_all_enabled=\${user_data \${destination_number}@\${domain_name} var forward_all_enabled} | | TRUE | 1 | 35                    |
| action              | set                  | forward_all_destination=\${user_data \${destination_number}@\${domain_name} var forward_all_destination} | | TRUE | 1 | 40      |
| action              | set                  | forward_busy_enabled=\${user_data \${destination_number}@\${domain_name} var forward_busy_enabled} | | TRUE | 1 | 45    |
| action              | set                  | forward_busy_destination=\${user_data \${destination_number}@\${domain_name} var forward_busy_destination} | | TRUE | 1 | 50  |
| action              | set                  | forward_no_answer_enabled=\${user_data \${destination_number}@\${domain_name} var forward_no_answer_enabled} | | TRUE | 1 | 55 |
| action              | set                  | forward_no_answer_destination=\${user_data \${destination_number}@\${domain_name} var forward_no_answer_destination} | | TRUE | 1 | 60 |
| action              | set                  | forward_user_not_registered_enabled=\${user_data \${destination_number}@\${domain_name} var forward_user_not_registered_enabled} | | TRUE | 1 | 65 |
| action              | set                  | forward_user_not_registered_destination=\${user_data \${destination_number}@\${domain_name} var forward_user_not_registered_destination} | | TRUE | 1 | 70 |
| action              | set                  | do_not_disturb=\${user_data \${destination_number}@\${domain_name} var do_not_disturb} | | TRUE | 1 | 75                    |
| action              | set                  | call_timeout=\${user_data \${destination_number}@\${domain_name} var call_timeout} | | TRUE | 1 | 80                    |
| action              | set                  | missed_call_app=\${user_data \${destination_number}@\${domain_name} var missed_call_app} | | TRUE | 1 | 85                    |
| action              | set                  | missed_call_data=\${user_data \${destination_number}@\${domain_name} var missed_call_data} | | TRUE | 1 | 90                    |
| action              | set                  | toll_allow=\${user_data \${destination_number}@\${domain_name} var toll_allow} | | TRUE | 1 | 95                    |
| action              | set                  | call_screen_enabled=\${user_data \${destination_number}@\${domain_name} var call_screen_enabled} | | TRUE | 1 | 100                   |

### Caller Details

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data          | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|-------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           |                      |                               | never                 |                        | 0                     | 5                     |
| action              | set                  | caller_destination=\${destination_number} |             | TRUE                   | 0                     | 10                    |
| action              | set                  | caller_id_name=\${caller_id_name} |                  | TRUE                   | 0                     | 15                    |
| action              | set                  | caller_id_number=\${caller_id_number} |              | TRUE                   | 0                     | 20                    |

### Call Direction

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|---------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | \${call_direction}    | ^(inbound\|outbound\|local)$ | never               |                        | 0                     | 5                     |
| anti-action         | export               | call_direction=local      |                       |                        | 0                     | 10                    |

### Variables

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                       | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|--------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           |                      |                                            |                       |                        | 0                     | 5                     |
| action              | export               | origination_callee_id_name=\${destination_number} |                  |                        | 0                     | 10                    |
| action              | set                  | RFC2822_DATE=\${strftime(%a, %d %b %Y %T %z)} |                    |                        | 0                     | 15                    |

### Call Limit

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|-------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | \${call_direction}    | ^(inbound\|outbound)$                     |                       |                        | 0                     | 5                     |
| action              | limit                | hash inbound \${domain_uuid} \${max_calls} !USER_BUSY |             |                        | 0                     | 10                    |

### Is Local

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | \${user_exists}       | FALSE                |                       |                        | 0                     | 5                     |
| action              | lua                  | app.lua is_local     |                       |                        | 0                     | 10                    |

### User Record

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                                      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|-----------------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           |                      |                                                           |                       |                        | 0                     | 5                     |
| action              | set                  | user_record=\${user_data \${destination_number}@\${domain_name} var user_record} | | TRUE | 0                     | 10                    |
| action              | set                  | from_user_exists=\${user_exists id \${sip_from_user} \${sip_from_host}} | | TRUE | 0                     | 15                    |
| condition           | \${user_exists}       | ^true$                                                    | never                 |                        | 1                     | 25                    |
| condition           | \${user_record}       | ^all$                                                     | never                 |                        | 1                     | 30                    |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 1                     | 35                    |
| condition           | \${user_exists}       | ^true$                                                    | never                 |                        | 2                     | 45                    |
| condition           | \${call_direction}    | ^inbound$                                                 | never                 |                        | 2                     | 50                    |
| condition           | \${user_record}       | ^inbound$                                                 | never                 |                        | 2                     | 55                    |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 2                     | 60                    |
| condition           | \${user_exists}       | ^true$                                                    | never                 |                        | 3                     | 70                    |
| condition           | \${call_direction}    | ^outbound$                                                | never                 |                        | 3                     | 75                    |
| condition           | \${user_record}       | ^outbound$                                                | never                 |                        | 3                     | 80                    |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 3                     | 85                    |
| condition           | \${user_exists}       | ^true$                                                    | never                 |                        | 4                     | 95                    |
| condition           | \${call_direction}    | ^local$                                                   | never                 |                        | 4                     | 100                   |
| condition           | \${user_record}       | ^local$                                                   | never                 |                        | 4                     | 105                   |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 4                     | 110                   |
| condition           | \${from_user_exists}  | ^true$                                                    | never                 |                        | 5                     | 120                   |
| action              | set                  | from_user_record=\${user_data \${sip_from_user}@\${sip_from_host} var user_record} | | TRUE | 5                     | 125                   |
| condition           | \${from_user_exists}  | ^true$                                                    | never                 |                        | 6                     | 135                   |
| condition           | \${from_user_record}  | ^all$                                                     | never                 |                        | 6                     | 140                   |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 6                     | 145                   |
| condition           | \${from_user_exists}  | ^true$                                                    | never                 |                        | 7                     | 155                   |
| condition           | \${call_direction}    | ^inbound$                                                 | never                 |                        | 7                     | 160                   |
| condition           | \${from_user_record}  | ^inbound$                                                 | never                 |                        | 7                     | 165                   |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 7                     | 170                   |
| condition           | \${from_user_exists}  | ^true$                                                    | never                 |                        | 8                     | 180                   |
| condition           | \${call_direction}    | ^outbound$                                                | never                 |                        | 8                     | 185                   |
| condition           | \${from_user_record}  | ^outbound$                                                | never                 |                        | 8                     | 190                   |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 8                     | 195                   |
| condition           | \${from_user_exists}  | ^true$                                                    | never                 |                        | 9                     | 205                   |
| condition           | \${call_direction}    | ^local$                                                   | never                 |                        | 9                     | 210                   |
| condition           | \${from_user_record}  | ^local$                                                   | never                 |                        | 9                     | 215                   |
| action              | set                  | record_session=true                                       |                       | TRUE                   | 9                     | 220                   |
| condition           | \${record_session}    | ^true$                                                    |                       |                        | 10                    | 230                   |
| action              | set                  | record_path=\${recordings_dir}/\${domain_name}/archive/\${strftime(%Y)}/\${strftime(%b)}/\${strftime(%d)} | | TRUE | 10                    | 235                   |
| action              | set                  | record_name=\${uuid}.\${record_ext}                         |                       | TRUE                   | 10                    | 240                   |
| action              | set                  | recording_follow_transfer=true                             |                       | TRUE                   | 10                    | 245                   |
| action              | set                  | record_append=true                                        |                       | TRUE                   | 10                    | 250                   |
| action              | set                  | record_in_progress=true                                   |                       | TRUE                   | 10                    | 255                   |
| action              | record_session       | \${record_path}/\${record_name}                             |                       | FALSE                  | 10                    | 260                   |

### Redial

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                              | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|---------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^(redial\|\*870)$                                 | on-true               |                        | 0                     | 5                     |
| action              | transfer             | \${hash(select/\${domain_name}-last_dial/\${caller_id_number})} |            |                        | 0                     | 10                    |
| condition           |                      |                                                   | never                 |                        | 1                     | 20                    |
| action              | hash                 | insert/\${domain_name}-last_dial/\${caller_id_number}/\${destination_number} | |       | 1                     | 25                    |

### Speed Dial

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*0(.*)$            |                       |                        | 0                     | 5                     |
| action              | lua                  | app.lua speed_dial $1 |                      |                        | 0                     | 10                    |

### Default Caller ID

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                             | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|--------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | \${emergency_caller_id_number} | ^$                                      | never                 |                        | 0                     | 5                     |
| action              | set                  | emergency_caller_id_name=\${default_emergency_caller_id_name} | | TRUE  | 0                     | 10                    |
| action              | set                  | emergency_caller_id_number=\${default_emergency_caller_id_number} | | TRUE | 0                    | 15                    |
| condition           | \${outbound_caller_id_number} | ^$                                       | never                 |                        | 1                     | 25                    |
| action              | set                  | outbound_caller_id_name=\${default_outbound_caller_id_name} | | TRUE  | 1                     | 30                    |
| action              | set                  | outbound_caller_id_number=\${default_outbound_caller_id_number} | | TRUE | 1                   | 35                    |

### Group Intercept

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*8$                |                       |                        | 0                     | 5                     |
| condition           | \${sip_h_X-intercept_uuid} | ^(.+)$          | on-true               |                        | 0                     | 10                    |
| action              | intercept            | $1                   |                       |                        | 0                     | 15                    |
| condition           |                      |                      |                       |                        | 1                     | 25                    |
| action              | answer               |                      |                       |                        | 1                     | 30                    |
| action              | lua                  | intercept_group.lua  |                       |                        | 1                     | 35                    |

### Conf Xfer

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                                      | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|-----------------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^conf_add_begin$                                          | on-true               |                        | 0                     | 5                     |
| action              | set                  | api_result=\${conference(\${conf_xfer_number} unmute \${conference_member_id} quiet)} | | | 0                     | 10                    |
| action              | bind_digit_action    | conf-xfer,*0,api:lua,transfer2.lua \${uuid} conf_enter_number::XML::conf-xfer@\${domain_name} conf_enter_to::XML::conf-xfer@\${domain_name} | | | 0 | 15                    |
| action              | bind_digit_action    | conf-xfer,##,api:lua,transfer2.lua \${uuid} conf_enter_number::XML::conf-xfer@\${domain_name} ::KILL:: | | | 0                     | 20                    |
| action              | bind_digit_action    | conf-xfer,*#,api:lua,transfer2.lua \${uuid} conf_add_end::XML::conf-xfer@\${domain_name} ::KILL:: | | | 0                     | 25                    |
| action              | bind_digit_action    | conf,*#,exec:execute_extension,conf_add_begin XML conf-xfer@\${domain_name} | | | 0                     | 30                    |
| action              | bind_digit_action    | none,NONE,api:sleep,1                                     |                       |                        | 0                     | 35                    |
| action              | set                  | continue_on_fail=true                                     |                       |                        | 0                     | 40                    |
| action              | transfer             | conf_enter_number XML conf-xfer@\${domain_name}            |                       |                        | 0                     | 45                    |
| condition           | destination_number   | ^conf_add_end$                                            | on-true               |                        | 1                     | 55                    |
| action              | digit_action_set_realm | conf                                                    |                       |                        | 1                     | 60                    |
| action              | set                  | api_result=\${conference(\${conf_xfer_number} mute \${conference_member_id})} | | | 1                     | 65                    |
| action              | conference           | \${conf_xfer_number}@page                                  |                       |                        | 1                     | 70                    |
| condition           | destination_number   | ^conf_enter_number$                                       | on-true               |                        | 2                     | 80                    |
| action              | digit_action_set_realm | none                                                    |                       |                        | 2                     | 85                    |
| action              | read                 | 2 11 'tone_stream://%(10000,0,350,440)' target_num 30000 # | |                      | 2                     | 90                    |
| action              | execute_extension    | conf_bridge_\${target_num} XML conf-xfer@\${domain_name}    |                       |                        | 2                     | 95                    |
| condition           | destination_number   | ^conf_bridge_$                                            | on-true               |                        | 3                     | 105                   |
| action              | execute_extension    | conf_add_end XML conf-xfer@\${domain_name}                 |                       |                        | 3                     | 110                   |
| condition           | destination_number   | ^conf_bridge_\*$                                          | on-true               |                        | 4                     | 120                   |
| action              | execute_extension    | conf_add_end XML conf-xfer@\${domain_name}                 |                       |                        | 4                     | 125                   |
| condition           | destination_number   | ^conf_bridge_(\d{2,7})$                                   | on-true               |                        | 5                     | 135                   |
| action              | digit_action_set_realm | conf-xfer                                               |                       |                        | 5                     | 140                   |
| action              | bridge               | {conf_xfer_number=\${conf_xfer_number},transfer_after_bridge=conf_enter_to:XML:conf-xfer@\${domain_name}}user/$1@\${domain_name} | | | 5                     | 145                   |
| action              | execute_extension    | conf_enter_number XML conf-xfer@\${domain_name}            |                       |                        | 5                     | 150                   |
| condition           | destination_number   | ^conf_bridge_                                             | on-true               |                        | 6                     | 160                   |
| action              | playback             | voicemail/vm-that_was_an_invalid_ext.wav                  |                       |                        | 6                     | 165                   |
| action              | execute_extension    | conf_enter_number XML conf-xfer@\${domain_name}            |                       |                        | 6                     | 170                   |
| condition           | destination_number   | ^conf_enter_to$                                           | on-true               |                        | 7                     | 180                   |
| action              | unbind_meta_app      |                                                           |                       |                        | 7                     | 185                   |
| action              | bind_digit_action    | conf,*#,exec:execute_extension,conf_add_begin XML conf-xfer@\${domain_name} | | | 7                     | 190                   |
| action              | digit_action_set_realm | conf                                                    |                       |                        | 7                     | 195                   |
| action              | answer               |                                                           |                       |                        | 7                     | 200                   |
| action              | playback             | tone_stream://L=1;%(500, 0, 640)                         |                       |                        | 7                     | 205                   |
| action              | conference           | \${conf_xfer_number}@page                                  |                       |                        | 7                     | 210                   |
| condition           | destination_number   | ^conf_xfer_from_dialplan$                                 |                       |                        | 8                     | 220                   |
| action              | lua                  | transfer2.lua \${uuid} conf_add_begin::XML::conf-xfer@\${domain_name} conf_enter_to::XML::conf-xfer@\${domain_name} | | | 8                     | 225                   |

### Page Extension

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*8(\d{2,7})$       |                       |                        | 0                     | 5                     |
| action              | set                  | destinations=$1      |                       |                        | 0                     | 10                    |
| action              | set                  | pin_number=87462988  |                       |                        | 0                     | 15                    |
| action              | set                  | mute=true            |                       |                        | 0                     | 20                    |
| action              | set                  | moderator=false      |                       |                        | 0                     | 25                    |
| action              | lua                  | page.lua             |                       |                        | 0                     | 30                    |

### Eavesdrop

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*33(\d{2,7})$      |                       |                        | 0                     | 5                     |
| action              | answer               |                      |                       |                        | 0                     | 10                    |
| action              | set                  | pin_number=03667751  |                       |                        | 0                     | 15                    |
| action              | lua                  | eavesdrop.lua $1     |                       |                        | 0                     | 20                    |

### Call Return

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data                              | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|---------------------------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*69$                                            |                       |                        | 0                     | 5                     |
| action              | transfer             | \${hash(select/\${domain_name}-call_return/\${caller_id_number})} |          |                        | 0                     | 10                    |

### Extension Queue

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data             | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*800(.*)$                      |                       |                        | 0                     | 5                     |
| action              | set                  | fifo_music=$\${hold_music}        |                       |                        | 0                     | 10                    |
| action              | set                  | extension_queue=queue_$1@\${domain_name} |                |                        | 0                     | 15                    |
| action              | set                  | fifo_simo=1                      |                       |                        | 0                     | 20                    |
| action              | set                  | fifo_timeout=30                  |                       |                        | 0                     | 25                    |
| action              | set                  | fifo_lag=10                      |                       |                        | 0                     | 30                    |
| action              | set                  | fifo_destroy_after_use=true      |                       |                        | 0                     | 35                    |
| action              | set                  | fifo_extension_member=$1@\${domain_name} |                |                        | 0                     | 40                    |
| action              | lua                  | extension_queue.lua              |                       |                        | 0                     | 45                    |

### Wake Up

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition           | destination_number   | ^\*(925)$            |                       |                        | 0                     | 5                     |
| action              | answer               |                      |                       |                        | 0                     | 10                    |
| action              | set                  | pin_number=14509639  |                       |                        | 0                     | 15                    |
| action              | set                  | time_zone_offset=-7  |                       |                        | 0                     | 20                    |
| action              | lua                  | wakeup.lua           |                       |                        | 0                     | 25                    |

### dx

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^dx$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | read | 11 11 'tone_stream://%(10000,0,350,440)' digits 5000 # | | | 0 | 15 |
| action | transfer | -bleg \${digits} | | | 0 | 20 |

### ATT Xfer

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^att_xfer$ | | | 0 | 5 |
| action | read | 2 6 'tone_stream://%(10000,0,350,440)' digits 30000 # | | | 0 | 10 |
| action | set | origination_cancel_key=# | | | 0 | 15 |
| action | att_xfer | user/\${digits}@\${domain_name} | | | 0 | 20 |

### Evesdrop

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*33(\d{2,7})$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | set | pin_number=03667751 | | | 0 | 15 |
| action | lua | eavesdrop.lua $1 | | | 0 | 20 |

### Please Hold

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | \${user_exists} | ^true$ | | | 0 | 5 |
| action | set | transfer_ringback=$\${hold_music} | | | 0 | 10 |
| action | answer | | | | 0 | 15 |
| action | sleep | 1500 | | | 0 | 20 |
| action | playback | ivr/ivr-hold_connect_call.wav | | | 0 | 25 |

### Cluecon Weekly

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^\*9(888\|8888\|1616\|3232)$ | | | 0 | 5 |
| action | export | hold_music=silence | | | 0 | 10 |
| action | bridge | sofia/\${use_profile}/$1@conference.freeswitch.org | | | 0 | 15 |

### Bind Digit Action

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | \${sip_authorized} | TRUE | never | | 0 | 5 |
| action | set | bind_target=both | | TRUE | 0 | 10 |
| anti-action | set | bind_target=peer | | TRUE | 0 | 15 |
| condition | | | | | 1 | 25 |
| action | bind_digit_action | local,*1,exec:execute_extension,dx XML \${context},\${bind_target} | | | 1 | 30 |
| action | bind_digit_action | local,*2,exec:record_session,$\${recordings_dir}/\${domain_name}/archive/\${strftime(%Y)}/\${strftime(%b)}/\${strftime(%d)}/\${uuid}.\${record_ext},\${bind_target} | | | 1 | 35 |
| action | bind_digit_action | local,*3,exec:execute_extension,cf XML \${context},\${bind_target} | | | 1 | 40 |
| action | bind_digit_action | local,*4,exec:execute_extension,att_xfer XML \${context},\${bind_target} | | | 1 | 45 |
| action | digit_action_set_realm | local | | | 1 | 50 |

### cf

| Dialplan Detail Tag | Dialplan Detail Type | Dialplan Detail Data | Dialplan Detail Break | Dialplan Detail Inline | Dialplan Detail Group | Dialplan Detail Order |
|---------------------|----------------------|----------------------|-----------------------|------------------------|-----------------------|-----------------------|
| condition | destination_number | ^cf$ | | | 0 | 5 |
| action | answer | | | | 0 | 10 |
| action | transfer | -both 30\${dialed_extension:2} XML \${context} | | | 0 | 15 |
