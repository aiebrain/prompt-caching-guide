# Prompt Caching Guide Skill

This skill teaches you how to reduce Claude API costs by up to 90% using prompt caching.

## When to Use
- User asks about prompt caching, Claude API costs, or token optimization
- User wants to reduce AI usage costs
- User asks about claude cache, cache_control, or automatic caching
- User is setting up OpenClaw cron jobs and wants to optimize them

## Key Facts

### What is Prompt Caching?
Prompt caching stores previously processed prompt content so it can be reused in subsequent requests — saving time and money.

### Cost Structure (2026)
| Token Type | Price vs Normal | Notes |
|---|---|---|
| Cache write (first request) | 125% | Only paid once |
| **Cache read (hit)** | **10%** | **90% savings ← key** |
| Normal Input | 100% | Baseline |
| Output | 100% | Unchanged |

### Auto Caching (2026 Update)
Anthropic released automatic caching in 2026 — **no code changes needed**.

Supported models:
- Claude Opus 4
- Claude Sonnet 4  
- Claude Sonnet 3.7 / 3.5
- Claude Haiku 3.5

### Core Principle
**Fixed content first, variable content last.**

```
┌─────────────────────────────┐
│ FIXED (cached)              │ ← Processed once
│ - Role / persona            │
│ - Rules / constraints       │
│ - Background info / examples│
├─────────────────────────────┤
│ VARIABLE (processed fresh)  │ ← Every request
│ - Today's date              │
│ - User question             │
│ - Real-time data            │
└─────────────────────────────┘
```

### ❌ Bad Pattern
```
"오늘 날짜는 2026-03-06이고,
너는 마케팅 전문가야.
규칙: 한국어로 답해라..."
// 날짜가 매일 바뀌면 전체 캐시 무효!
```

### ✅ Good Pattern
```
"너는 마케팅 전문가야.
규칙: 한국어로 답해라
규칙: 500자 이내로...
← 여기까지 고정 (캐시됨)

오늘 날짜: 실행 시 계산
질문: [사용자 입력]"
```

### OpenClaw Cron Job Pattern
```
【역할】 매일 09:00 실행되는 모닝 브리핑이다.   ← 고정

【작업】
1. GitHub 커밋 확인
2. 어제 memory 파일 읽기
3. 텔레그램 전송

【형식】 300자 이내, 이모지 포함             ← 고정

【필수 규칙】 exec+PowerShell만 사용         ← 고정
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
변동 데이터(날짜)는 실행 시점에 계산         ← 변동
```

### CLAUDE.md Usage (Claude Code)
```markdown
# CLAUDE.md — Project Rules (fixed context)
# This file loads identically every request → maximizes cache hits

## Store Info
- Target: 30-40s housewives
- Tone: Friendly and trustworthy
- Pricing: Always -5% vs competitors

## Work Rules
- Write in Korean
- Always log errors
```

## FAQ

**Q: Do I need to change my code?**
No. Anthropic's 2026 auto-caching handles it automatically. Optimizing your prompt structure improves hit rates further.

**Q: How do I know if caching is working?**
Check `cache_read_input_tokens` in the Claude API response. The higher this value, the more you saved.

**Q: When is caching most effective?**
1. Daily repeating cron jobs
2. Apps with identical system prompts
3. Claude Code projects using CLAUDE.md
4. Repeated analysis of long documents

**Q: How long does the cache last?**
Default 5 minutes. Can be extended up to 1 hour with configuration.

**Q: Is the cache shared with other users?**
No. Cache is scoped to the same API key only. Completely secure.

## Resources
- [Anthropic Official Docs](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)
- [Anthropic Cookbook (GitHub)](https://github.com/anthropics/anthropic-cookbook)
- [Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)
- [Guide Page](https://aiebrain.github.io/prompt-caching-guide/)
