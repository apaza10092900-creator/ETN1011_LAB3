# Laboratorio — Enlaces Internacionales de Internet en Bolivia

**Estudiante:** Apaza Queso Gaston Jhonny 
**Fecha de consulta:** 2026-11-06  
**Nota de método:** la investigación de §1–§3 y §5 se hizo con búsqueda web y fuentes secundarias (agregadores de BGP, prensa, PeeringDB/Pulse). Las herramientas interactivas (UIT Broadband Maps, ATT SMI, Submarine Cable Map) y las mediciones `dig`/`ping` **se ejecuto** con normalidad

---

## 1. Infraestructura de fibra

| Infraestructura identificada | Ubicación / extremos publicados | Fuente (URL) |
|---|---|---|
| Fibra terrestre 1 | La Paz – Desaguadero (Bolivia) → Ilo/Moquegua (Perú), conexión inaugurada en 2020; Perú facilita acceso a la red del Pacífico. Condición: operativa según prensa. | https://www.telesemana.com/blog/2020/09/01/tras-cuatro-anos-y-cambios-en-la-agenda-bolivia-promete-inaugurar-su-conexion-de-fibra-al-pacifico-esta-semana/ |
| Fibra terrestre 2 | La Paz – Tambo Quemado (Bolivia) → Arica (Chile); descrito como enlace de redundancia al principal por Perú (Lurín). Nota: en oct-2026 Bolivia y Chile acordaron *estudiar* un corredor adicional Arica–Visviri/Charaña (no existente aún). | https://paradigma.emi.edu.bo/index.php/aa/article/download/33/36/64 · https://www.telesemana.com/blog/2026/10/19/chile-y-bolivia-acuerdan-estudiar-un-corredor-de-fibra-optica-entre-el-pacifico-y-el-atlantico/ |
| Cable submarino 1 | **SAm-1** (Pacífico y Atlántico, anillo). Estación de amarre: **Arica, Chile** (también Las Toninas, Argentina, en el Atlántico). | https://fcc.report/IBFS/SCL-MOD-20180905-00032/1982583.pdf |
| Cable submarino 2 | **Mistral** (costa Pacífico). Estación de amarre: **Lurín, Perú** (también Arica y Valparaíso, Chile). | https://convergedigest.com/telxius-activates-mistral-along-latin/?amp=1 |

**Contraste UIT vs. ATT:** red troncal de fibra de 26.587 km en 2020 (https://internetbolivia.org/file/2021/11/guia_acceso_internet.pdf). Verificar en https://bbmaps.itu.int/bbmaps/ y https://smi.att.gob.bo/ y anotar la fecha del documento.

**Diferencia (qué muestra y qué no):** los mapas muestran tramos, localidades y estaciones de amarre publicadas; **no demuestran** qué ISP boliviano usa qué cable o tramo, ni qué capacidad contrata, ni la ruta física real de su tráfico.

---

## 2. ASN y proveedores

| Red / servicio | ASN encontrado | Titular o nombre visible | Upstream probable (solo ISP) |
|---|---|---|---|
| ENTEL | AS6568 | Entel S.A. – EntelNet / Empresa Nacional de Telecomunicaciones | Level 3 (AS3356), Telecom Italia Sparkle, Telefónica Global Solutions, ANTEL (Uruguay) — *probable* |
| AXS | AS26210 | AXS Bolivia S.A. | no verificado |
| COMTECO | AS27839 (también AS52340) | Comteco Ltda | no verificado |
| COTAS | AS25620 | COTAS LTDA. | no verificado |
| TIGO | AS27882 | Telefónica Celular de Bolivia S.A. | Telefónica Global Solutions, Internexa Perú, Telecel (Paraguay), SAMM (Brasil) — *probable* |
| Google / YouTube | AS15169 / AS36040 | Google LLC / YouTube | — |
| Netflix | AS2906 | Netflix Streaming Services Inc. | — |
| Meta / Facebook | AS32934 | Facebook, Inc. | — |
| TikTok / ByteDance | AS138699 (TikTok Pte. Ltd., SG) y AS396986 (ByteDance Inc., US); además AS150436 (BytePlus) | No hay ASN único: son varias entidades de ByteDance | — |

**Verificación en LACNIC RDAP (2 ASN):** PENDIENTE. Hacer en https://rdap-web.lacnic.net/ con AS27839 (Comteco) y AS25620 (COTAS), y comparar el titular. Observación: un agregador registra AS6568 bajo ARIN (no LACNIC), por lo que ENTEL puede no aparecer en RDAP de LACNIC.

**Upstreams:** el campo "Receiving From" de BigDataCloud (sin fecha clara) lista esos proveedores; es evidencia de *relación observada*, no de contrato. Confirmar en HE BGP / BGP.Tools.

Fuentes: https://bgp.tools/ixp/PIT%20Bolivia · https://bgp.he.net/country/BO · https://db-ip.com/country/BO · https://bigdatacloud.com/asn-lookup/AS6568 · https://bigdatacloud.com/asn-lookup/AS27882 · https://www.ipinfo.io/AS36040 · https://doc.rrflow.com.br/en/scripts · https://www.bigdatacloud.com/network-lookup/139.177.225.0/24 · https://bgp.he.net/irr/as-set/AS-BYTED

---

## 3. Peering, tránsito y BGP

| Relación entre ASN | Tipo | Evidencia (herramienta/URL) |
|---|---|---|
| AS6568 (Entel) ← AS3356 (Level 3) | tránsito probable | BigDataCloud "Receiving From": https://bigdatacloud.com/asn-lookup/AS6568 |
| AS6568 (Entel) ↔ AS27882 (Tigo), AS26210, AS27839, AS25620… | peering probable (misma LAN de IXP, no prueba sesión activa) | PIT Bolivia en BGP.Tools: https://bgp.tools/ixp/PIT%20Bolivia |
| AS6568 → AS13335 (Cloudflare) | incierto | BigDataCloud "Transit To": https://bigdatacloud.com/asn-lookup/AS6568 |

**PeeringDB:** Entel (política *Selective*), AXS (*Open*), COTAS (*Open*) figuran en el IXP **PIT Bolivia** (La Paz). Entre los miembros del PIT hay redes de contenido/edge como **EdgeUno (AS64124)** y Alibaba Cloud, pero **no aparecen Google, Netflix, Meta ni TikTok** en esa lista (consulta 2026-11-06). Fuentes: https://pulse.internetsociety.org/en/ixp-tracker/ixp/921 · https://bgp.tools/ixp/PIT%20Bolivia

**AS_PATH en RIPEstat:** no observable (PENDIENTE: consultar un prefijo, p. ej. de AS6568, en https://stat.ripe.net/ y copiar un AS_PATH).

**En una frase:** aparecer como vecino BGP solo indica que dos redes intercambian anuncios; no demuestra un contrato comercial (tránsito vs. peering gratuito) ni por dónde pasa físicamente la fibra.

---

## 4. DNS y latencia

ejecutaremos dig y ping con el script.

```bash
#!/usr/bin/env bash
# lab_dns.sh — genera filas de tabla para §4
DOMS=("impuestos.bo" "umsa.bo" "entel.bo" "EMPRESA.com.bo")   # ISP elegido: ajústalo (p. ej. dominio oficial de Entel o Tigo)
echo "Fecha: $(date -u +%F)"
for d in "${DOMS[@]}"; do
  echo "== $d =="
  dig "$d" A  +noall +answer
  dig "$d" NS +noall +answer
  dig "$d" MX +noall +answer
done
for d in impuestos.bo umsa.bo; do
  dig "$d" AAAA +noall +answer
  dig "$d" SOA  +noall +answer
  dig "$d" TXT  +noall +answer
done
dig @1.1.1.1 impuestos.bo A +noall +answer
for d in impuestos.bo umsa.bo entel.bo; do
  echo "== ping $d =="; ping -c 5 "$d" | tail -3
done
```

| Dominio | A (IP o sin respuesta) | NS (uno) | MX (uno o ausente) | TTL (s) |
|---|---|---|---|---|
| impuestos.bo | 170.82.244.41 | no visible | no visible | 288 |
| umsa.bo | 200.7.161.56 | jupiter.umsa.bo | 10 alt4.aspmx.l.google.com. | 2995 |
| entel.bo | 200.87.142.78 | ns.entelnet.bo | bo.mail.protection.outlook.com | 60 |
| tigo.com.bo | 52.0.229.131 | dns2.wimaxtigo.bo | 10 tigo-com-bo.mail.protection.outlook.com | 300 |

| Dominio | IP resuelta | RTT promedio (ms) | Pérdida (%) | Observación |
|---|---|---|---:|---|
| impuestos.bo |170.82.244.41  | sin valor | sin valor | sin respuesta |
| umsa.bo | 200.7.161.56 | 50.6 ms |  |min/avg/max/mdev = 43.228/52.486/69.017/9.514 ms |


**¿DNS permite saber por sí solo en qué país está alojado el servicio?** No: DNS solo devuelve nombres y direcciones; la IP puede ser de un CDN/proxy (anycast) o estar geolocalizada de forma imprecisa, y el RTT no equivale a distancia física.

---

## 5. CDN usadas por servicios populares

| Servicio | ISP boliviano 1 (ENTEL): evidencia | ISP boliviano 2 (TIGO): evidencia | ¿Caché dentro del ISP demostrada? |
|---|---|---|---|
| Netflix (AS2906) | no verificable (no está en PIT Bolivia; sin fuente específica de Open Connect en Bolivia) | no verificable | No |
| Google / YouTube (AS15169/AS36040) | Indicio de tercero: Netify lista 81 IPs de "Google Edge CDN" en el ASN de Entel (La Paz / Santa Cruz). Clasificación: **no verificable** por fuente oficial | no verificable (sin dato específico) | No demostrada (indicio solo para Entel) |
| TikTok (AS138699/AS396986) | no verificable | no verificable | No |
| Facebook / Meta (AS32934) | no verificable | no verificable | No |

Fuente del indicio de Google: https://www.netify.ai/resources/cdn/google-cloud-cdn/country/bo (dataset de terceros, puede tener hasta un mes de antigüedad).

| CDN comercial | ASN o red encontrada | Servicio / característica identificada |
|---|---|---|
| Cloudflare | AS13335 | Red anycast; CDN Planet reporta 1 PoP en La Paz. https://www.cdnplanet.com/geo/bolivia-cdn/ |
| Fastly | AS54113 | Sin PoP boliviano hallado; el más cercano listado es Lima (Perú). https://www.netify.ai/resources/networks/fastly/country/pe |
| Akamai | AS20940 | Servidores ubicados dentro de redes de ISP; Netify detecta IPs de Akamai en redes de La Paz y en COTAS (AS25620), sin detalle oficial. https://www.netify.ai/resources/cdn/akamai/country/bo |

**Pregunta final:** para afirmar que un ISP tiene caché Netflix/Google dentro de su red haría falta una fuente específica (anuncio del ISP o del proveedor, o el hostname del servidor de caché, p. ej. `*.googlevideo.com` o `*.oca.nflxvideo.net`, resolviendo a IPs del ASN del ISP, más trazas/RTT consistentes). Una IP de CDN no demuestra ubicación física porque el anycast y las geobases de IP no revelan dónde está el servidor.

---

## 6. Observaciones

1. **Ruta principal:** la conectividad internacional descrita pasa por Perú (Desaguadero → Ilo/Lurín) con Chile (Tambo Quemado → Arica) como redundancia; los cables submarinos relevantes (SAm-1, Mistral) tocan tierra en Arica y Lurín, no en Bolivia. *(Fuentes en §1; el rol exacto por ISP es inferencia.)*
2. **IXP:** PIT Bolivia reúne a los principales ISP (Entel, Tigo, COMTECO, COTAS, AXS, Nuevatel) y a EdgeUno, pero no a Google, Netflix, Meta ni TikTok (2026-11-06). Eso no prueba que no tengan caché dentro de un ISP.
3. **Upstreams:** los datos de BigDataCloud sugieren tránsito de Level 3/Sparkle/Telefónica Global Solutions para Entel y de Telefónica Global Solutions/Internexa para Tigo; es **relación observada**, no contrato (inferencia).
4. **Evidencia de CDN:** solo hay un indicio de terceros (Netify) de caché Google en Entel; para Netflix, Meta y TikTok **no verificable**.
5. **Límites:** las mediciones DNS/ping y las verificaciones en UIT/ATT/RDAP/RIPEstat están pendientes y deben completarse antes de entregar.
