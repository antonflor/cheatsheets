# Cisco Unified Customer Voice Portal

> **Status:** Legacy reference — technical validation pending

Cisco Unified Customer Voice Portal (CVP) provides voice self-service and call-treatment functions within Cisco contact-center environments. Deployments commonly integrate SIP call control, VoiceXML gateways, media services, speech resources, and Cisco routing components.

## Typical components

- Call Server for call-control and application coordination;
- VoiceXML gateways for media and VoiceXML execution;
- media servers for prompts and static content;
- Operations, Administration, Maintenance, and Provisioning interfaces;
- integration with ICM/UCCE routing;
- speech-recognition or text-to-speech services where deployed;
- reporting and operational data consumed by other Cisco components.

Exact component names, supported deployment models, and scaling limits are release-specific.

## Operational checks

Before making a change, identify:

- the exact CVP, UCCE, Unified CM, IOS, and gateway releases;
- SIP call path and dial-peer design;
- VoiceXML application and media-server dependencies;
- redundancy model and active call impact;
- certificate, DNS, NTP, and authentication dependencies;
- current call volume, license or capacity limits, and maintenance window;
- rollback and configuration-backup procedure.

## Troubleshooting sequence

1. Reproduce one call and record timestamps, calling number, called number, and correlation identifiers.
2. Determine the last component that processed the call successfully.
3. Trace SIP signaling across ingress gateway, CVP, Unified CM, and routing components.
4. Verify media, VoiceXML, and speech-resource reachability separately from call signaling.
5. Compare routing-script decisions with CVP and gateway logs.
6. Check DNS, certificates, time synchronization, codec, DTMF, and network quality.
7. Test the least disruptive correction on a controlled call path.

## Common symptom areas

| Symptom | Checks |
|---|---|
| Call fails before IVR | SIP routing, dial peers, Call Server, routing response |
| Prompts are missing | Media URL, HTTP reachability, file format, cache, permissions |
| DTMF is not recognized | DTMF method, codec, gateway configuration, application logic |
| Speech recognition fails | Speech server reachability, licenses, grammar, latency |
| One-way or poor audio | RTP path, NAT, firewall, codec, packet loss, jitter |
| Intermittent call drops | SIP timers, gateway resources, network loss, component health |

## Related references

- [CUIC](cuic.md)
- [ICM](icm.md)

> [!NOTE]
> This page intentionally avoids version-specific configuration commands until they are revalidated against the Cisco documentation set matching the deployed release.
