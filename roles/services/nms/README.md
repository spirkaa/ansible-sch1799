# Роль Ansible: NMS

Роль Ansible, которая разворачивает LibreNMS и SmokePing в контейнерах Docker.

## Зависимости

Нет

## Шаблоны уведомлений (HTML) для Telegram и Microsoft Teams

### Default Alert Template

```html
<strong>{{ $alert->title }}</strong>
<pre>Host    : {{ $alert->hostname }} @ {{ $alert->location }}
sysName : {{ $alert->sysName }}
@if ($alert->state == 0)
Elapsed : {{ $alert->elapsed }}
@endif
Time    : {{ $alert->timestamp }}
UID     : {{ $alert->uid }}
Severity: {{ $alert->severity }}
Rule    : @if ($alert->name){{ $alert->name }} @else {{ $alert->rule }} @endif</pre>
@if ($alert->faults)
<pre>Faults  :
@foreach ($alert->faults as $key => $value)  #{{ $key }}: {{ $value['string'] }}
@endforeach</pre>@endif

@if ($alert->alert_notes)
<strong>Notes:</strong>
<pre>{{$alert->alert_notes}}</pre>
@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```

### Port Utilization

```html
<strong>{{ $alert->title }}</strong>
<pre>Host    : {{ $alert->hostname }} @ {{ $alert->location }}
sysName : {{ $alert->sysName }}
@if ($alert->state == 0)
Elapsed : {{ $alert->elapsed }}
@endif
Time    : {{ $alert->timestamp }}
UID     : {{ $alert->uid }}
Severity: {{ $alert->severity }}
Rule    : @if ($alert->name){{ $alert->name }} @else {{ $alert->rule }}@endif</pre>
@if ($alert->faults)
@foreach ($alert->faults as $key => $value)
<pre>  Int     : {{ $value['ifDescr'] }}
  Descr   : {{ $value['ifAlias'] }}
  Speed   : {{ ($value['ifSpeed']/1000000) }} Mbps
  Util In : {{ (($value['ifInOctets_rate']*8)/$value['ifSpeed'])*100 }} Mbps
  Util Out: {{ (($value['ifOutOctets_rate']*8)/$value['ifSpeed'])*100 }} Mbps</pre>
@endforeach @endif

@if ($alert->alert_notes)
<strong>Notes:</strong>
<pre>{{$alert->alert_notes}}</pre>
@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```

### Services Alert Template

```html
<strong>{{ $alert->title }}</strong>
<pre>@if ($alert->state == 0)
Elapsed : {{ $alert->elapsed }}
@endif
Time    : {{ $alert->timestamp }}
UID     : {{ $alert->uid }}
Severity: {{ $alert->severity }}</pre>
@if ($alert->faults)
<pre>Faults  :
@foreach ($alert->faults as $key => $value)
  {{ $value['service_type'] }}: {{ $value['service_desc'] }}
  {{ $value['service_message'] }}
@endforeach</pre>@endif

@if ($alert->alert_notes)
<strong>Notes:</strong>
<pre>{{$alert->alert_notes}}</pre>
@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```
