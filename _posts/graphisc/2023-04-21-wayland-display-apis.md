---
title: "Graphics: Wayland Display Server와 Client 간 통신"
categories:
  - graphics
tags:
  - wayland
  - graphics
  - compositor
  - wl_display_flush
  - wl_display_dispatch_pending
  - wl_display_sync
  - wl_display_roundtrip
comments: true
toc: true
toc_sticky: true
---

Wayland Display Server는 리눅스 그래픽 서브시스템에서 사용되는 Display Server입니다. Wayland은 X Window System과 달리 다양한 디바이스와 형식을 지원하며 더 나은 그래픽 성능을 제공합니다. 이러한 성능 향상은 Wayland Display Server와 클라이언트 간의 통신 방식에서 시작됩니다. Wayland Display Server와 클라이언트 간의 통신은 일반적으로 Wayland 프로토콜을 사용하여 이루어집니다.

# 주요 API

이번 글에서는 Wayland Display Server와 클라이언트 간의 통신에서 사용되는 여러 함수에 대해 살펴보겠습니다. 특히, **`wl_display_flush()`**, **`wl_display_dispatch_pending()`**, **`wl_display_sync()`**, **`wl_display_roundtrip()`** 함수에 대해 자세히 살펴보겠습니다.

- **`wl_display_flush`**는 보류 중인 요청을 서버로 보냅니다. 이 함수는 모든 보류 중인 요청을 서버로 보내고, 서버가 이를 인식한 후 반환됩니다. 일반적으로 모든 요청이 전송된 후 읽기 또는 쓰기 작업에서 차단되기 전에 모든 요청이 전송되었는지 확인하는 데 사용됩니다.

- **`wl_display_dispatch_pending`**은 서버에서 보류 중인 이벤트를 처리하는 데 사용됩니다. 이 함수는 대기 중인 이벤트를 검색하고 처리합니다. 이 함수는 블로킹되지 않으며, 대기 중인 이벤트가 없으면 즉시 반환됩니다.
- **`wl_display_sync`**는 클라이언트를 서버와 동기화하는 데 사용됩니다. 이 함수는 서버에 요청을 보내고 서버가 이전 요청을 모두 처리하고 응답을 보내기를 기다립니다. 이전 요청이 처리된 것을 보장해야 할 때 유용합니다.
- **`wl_display_roundtrip`**은 **`wl_display_flush`**와 **`wl_display_sync`**의 결합입니다. 이 함수는 서버에 요청을 보내고 서버가 요청을 처리하고 응답을 보내기를 기다립니다. 이 함수는 클라이언트가 진행하기 전에 서버에서 특정 응답을 기다려야 할 때 사용됩니다.

**`wl_display_sync`**와 **`wl_display_roundtrip`**을 서버 이벤트를 처리하는 동일한 스레드에서 호출하면 데드락이 발생할 수 있으므로이 함수를 별도의 스레드에서 호출하는 것이 좋습니다.

# API들 간의 차이점

아래는 각 함수의 역할과 차이점을 정리한 테이블입니다.

| 함수 이름 | 역할 | 차이점 |
| --- | --- | --- |
| wl_display_flush | 서버로 버퍼의 내용을 전송합니다. | flush 함수를 호출하면 클라이언트가 버퍼에 쓴 내용을 서버로 전송합니다. 서버는 이후에 wl_display_dispatch나 wl_display_dispatch_queue 함수를 호출하여 서버에서 온 이벤트를 처리할 수 있습니다. flush 함수는 서버로부터 ACK를 받은 후 반환됩니다. |
| wl_display_dispatch_pending | 큐(queue)에 남아있는 이벤트를 처리합니다. | dispatch_pending 함수를 호출하면 서버로부터 온 새로운 이벤트를 큐(queue)에서 가져와 처리합니다. 이 함수는 큐에 이벤트가 없을 때까지 계속 실행됩니다. 이벤트를 처리하는 동안 어떤 이벤트가 도착할 때까지 대기하지 않으므로 non-blocking 함수입니다. 다른 이벤트를 처리하다가 이벤트가 도착하면 바로 처리할 수 있습니다. |
| wl_display_sync | 서버로부터 ACK를 받을 때까지 대기합니다. | sync 함수를 호출하면 서버로부터 ACK를 받을 때까지 대기합니다. 서버는 모든 pending request에 대해 응답을 보내야 합니다. 따라서 sync 함수를 호출하면 서버는 모든 pending request를 처리하고 이에 대한 응답을 보내야 하므로, 모든 pending request를 처리한 후에 반환합니다. sync 함수는 blocking 함수이기 때문에, 클라이언트는 다른 작업을 할 수 없습니다. |
| wl_display_roundtrip | wl_display_flush와 wl_display_sync의 조합입니다. 서버로 요청(request)을 보내고, 서버로부터 응답(response)을 받을 때까지 대기합니다. | roundtrip 함수는 서버로 요청(request)을 보내고, 서버로부터 응답(response)을 받을 때까지 대기합니다. flush 함수를 호출하여 서버로 요청을 보내고, sync 함수를 호출하여 서버로부터 응답을 받습니다. 따라서 flush 함수와 sync 함수의 조합으로 생각할 수 있습니다. flush 함수와 sync 함수를 직접 호출하는 것보다 편리합니다. roundtrip 함수는 blocking 함수이기 때문에 , 클라이언트는 다른 작업을 할 수 없습니다. |

# 결론

요약하면, **`wl_display_flush`**는 보류 중인 요청이 모두 서버로 전송되었는지 보장하기 위해 사용됩니다. **`wl_display_dispatch_pending`**는 서버에서 보류 중인 이벤트를 처리하는 데 사용됩니다. **`wl_display_sync`**는 클라이언트를 서버와 동기화하는 데 사용되며, **`wl_display_roundtrip`**은 서버에서 특정 응답을 기다리는 데 사용됩니다. 이 네 가지 함수의 차이점과 제한 사항을 이해하는 것은 신뢰성이 높고 효율적인 Wayland 컴포지터 또는 클라이언트를 개발하는 데 중요합니다.