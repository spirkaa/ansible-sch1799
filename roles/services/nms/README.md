# Роль Ansible: NMS

Роль Ansible, которая разворачивает LibreNMS и SmokePing в контейнерах Docker.

## Зависимости

Нет

## Шаблоны уведомлений (HTML)

### Default Alert Template

```html
<strong>{{ $alert->title }}</strong>
<pre>Host    : {{ $alert->hostname }} @ {{ $alert->location }}</pre>
<pre>sysName : {{ $alert->sysName }}</pre>
@if ($alert->state == 0)
<pre>Elapsed : {{ $alert->elapsed }}</pre>
@endif
<pre>Time    : {{ $alert->timestamp }}</pre>
<pre>Rule    : @if ($alert->name){{ $alert->name }} @else {{ $alert->rule }} @endif</pre>
<pre>UID     : {{ $alert->uid }}</pre>
<pre>Severity: {{ $alert->severity }}</pre>
@if ($alert->faults)
<pre>Faults  :
@foreach ($alert->faults as $key => $value)  #{{ $key }}: {{ $value['string'] }}
@endforeach</pre>@endif

@if ($alert->alert_notes)
<strong>Notes:</strong>
{{$alert->alert_notes}}
@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```

### Port Utilization

```html
<strong>{{ $alert->title }}</strong>
<pre>Host    : {{ $alert->hostname }} @ {{ $alert->location }}</pre>
<pre>sysName : {{ $alert->sysName }}</pre>
@if ($alert->state == 0)
<pre>Elapsed : {{ $alert->elapsed }}</pre>
@endif
<pre>Time    : {{ $alert->timestamp }}</pre>
<pre>Rule    : @if ($alert->name){{ $alert->name }} @else {{ $alert->rule }} @endif</pre>
<pre>UID     : {{ $alert->uid }}</pre>
<pre>Severity: {{ $alert->severity }}</pre>

@if ($alert->faults)
@foreach ($alert->faults as $key => $value)
<pre>Int     : {{ $value['ifDescr'] }}</pre>
<pre>Descr   : {{ $value['ifAlias'] }}</pre>
<pre>Speed   : {{ ($value['ifSpeed']/1000000) }} Mbps</pre>
<pre>Util In : {{ (($value['ifInOctets_rate']*8)/$value['ifSpeed'])*100 }} Mbps</pre>
<pre>Util Out: {{ (($value['ifOutOctets_rate']*8)/$value['ifSpeed'])*100 }} Mbps</pre>
@endforeach @endif

@if ($alert->alert_notes)
<strong>Notes:</strong>
{{$alert->alert_notes}}
@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```

### Service Alert

```html
<strong>{{ $alert->title }}</strong>
@if ($alert->state == 0)
<pre>Elapsed : {{ $alert->elapsed }}</pre>
@endif
<pre>Time    : {{ $alert->timestamp }}</pre>
<pre>Severity: {{ $alert->severity }}; Unique-ID: {{ $alert->uid }}</pre>
@if ($alert->faults)
<pre>Faults  :
@foreach ($alert->faults as $key => $value)
  {{ $value['service_type'] }}: {{ $value['service_desc'] }}
  {{ $value['service_message'] }}
@endforeach</pre>@endif

@if ($alert->state != 0)
<strong><a href="https://nms.licey1799.ru/alerts">[Acknowledge Alert]</a></strong>
@endif
```
