# 01 — Email Agent

## Problem
Małe firmy i freelancerzy tracą czas na ręczne przeglądanie skrzynki. Każdy email wymaga oceny: czy jest pilny? Czy wymaga odpowiedzi? Czy to spam? To żmudna, powtarzalna praca która zajmuje od 30 do 60 minut dziennie.

## Rozwiązanie
Autonomiczny agent który monitoruje skrzynkę Gmail co 15 minut, klasyfikuje wiadomości przy pomocy AI i podejmuje automatyczne akcje — bez udziału człowieka.

**Trzy ścieżki działania:**
- **Urgent** — natychmiastowe powiadomienie na Telegram z nadawcą i tematem
- **Normal** — AI generuje draft odpowiedzi w tym samym wątku, gotowy do wysłania jednym kliknięciem
- **Spam** — wiadomość automatycznie trafia do folderu spam

## Architektura

```
Gmail Trigger (co 15 min)
    ↓
Text Classifier (Claude Haiku)
    ↓
urgent → Telegram Bot (powiadomienie na telefon)
normal → Basic LLM Chain → Gmail Draft (gotowa odpowiedź)
spam   → Gmail Label (przeniesienie do spamu)
```

## Stack
- **n8n** — orchestracja workflow
- **Claude Haiku** — klasyfikacja i generowanie odpowiedzi (Anthropic API)
- **Gmail OAuth2** — odczyt i zapis wiadomości
- **Telegram Bot API** — powiadomienia real-time

## Wartość biznesowa
- Eliminuje ręczne przeglądanie skrzynki
- Drafty odpowiedzi gotowe w sekundach, nie minutach
- Pilne sprawy nie giną w skrzynce — trafiają na telefon natychmiast
- Wdrożenie: ~2h, zero kodu po stronie klienta

## Decyzje architektoniczne
- **Haiku zamiast Sonnet** do klasyfikacji — 10x tańszy, wystarczający dla prostego zadania
- **Draft zamiast auto-send** — człowiek zatwierdza odpowiedź (Human-in-the-Loop)
- **Spam zamiast delete** — bezpieczeństwo przed false positive, klient ma wgląd
- **Polling co 15 minut** — balans między responsywnością a kosztem API

## Status
🔨 W budowie — wersja v1 działająca lokalnie
