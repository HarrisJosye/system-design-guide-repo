# Epilogue — The Staff/Principal Design Review Checklist

Across all four modules, the same questions separate senior designs from principal ones:

1. **Where is the source of truth, and what enforces its invariants?** It should be a constraint or conditional write, not a lock or a cache.
2. **Which operations are retried, and what makes each one idempotent?** Name the dedupe key and the transaction it commits in.
3. **What number fences stale actors?** A version, term, epoch or LSN, checked by the component being written to.
4. **What is the CP/AP choice per data type, and who signed off on it?**
5. **What are the capacity numbers?** Show them via Little's Law, bytes per unit, and fan-out math, not "it scales horizontally".
6. **What is the degradation ladder, and which user-visible behaviour does each rung produce?**
7. **What bounds the blast radius?** Cells, bulkheads and per-dependency breakers.
8. **How is correctness verified continuously in production?** Reconciliation jobs, invariant checks, golden-set evals.
9. **How fresh is each input to a decision, and what happens when it isn't?** Know the staleness of every feature and replica on the critical path.
10. **Can the change be reversed, and until when?** For migrations, models and schema changes, name the rollback mechanism and the point of no return.

## Further reading

These are well-known references for going deeper. Verify details against the current editions.

- Martin Kleppmann, *Designing Data-Intensive Applications*.
- Diego Ongaro and John Ousterhout, "In Search of an Understandable Consensus Algorithm" (the Raft paper).
- Martin Kleppmann, "How to do distributed locking" (the Redlock critique), and Salvatore Sanfilippo's response.
- Yu. A. Malkov and D. A. Yashunin, the HNSW paper ("Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs").
- Kwon et al., the vLLM / PagedAttention paper.
- Das, Gupta and Motivala, the SWIM membership protocol paper.
- Mohan et al., the ARIES recovery paper.
- Lamping and Veach, "A Fast, Minimal Memory, Consistent Hash Algorithm" (jump hash).
- Google SRE book chapters on handling overload and addressing cascading failures.
- Tyler Akidau et al., "The Dataflow Model" paper, and the book *Streaming Systems*.
- Carbone et al., "Lightweight Asynchronous Snapshots for Distributed Dataflows" (Flink checkpointing).
- Martin Fowler, "StranglerFigApplication" and related articles on legacy displacement.
- The independent review of the 2018 TSB migration, for a detailed account of a big-bang core migration failure.

---

[← Back to contents](../README.md)
