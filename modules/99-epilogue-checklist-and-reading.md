# Epilogue — The Staff/Principal Design Review Checklist

These questions separate good designs from great ones in every module:

1. **Where is the real data, and what keeps it correct?** The answer should be a database rule or a conditional write, not a lock or a cache.
2. **Which actions get retried, and what makes each one safe to repeat?** Name the duplicate-check key and the transaction it's saved in.
3. **What number blocks old actors?** A version, term, epoch or log position, checked by the thing being written to.
4. **For each type of data, did we choose consistency or availability, and who agreed?**
5. **What are the actual numbers?** Show the maths (Little's Law, bytes per item, fan-out), not just "it scales".
6. **What are the degradation steps, and what does the user see at each one?**
7. **How far can one failure spread?** Cells, separate pools, one breaker per dependency.
8. **How do we check correctness all the time in production?** Automatic reconciliation, rule checks, test question sets.
9. **How fresh is each piece of data used in a decision, and what happens when it's stale?** Know how out of date every feature and replica on the critical path can be.
10. **Can we undo this change, and until when?** For migrations, models and schema changes, name the undo method and the point of no return.

## Further reading

These are well-known references for going deeper. Check details against the latest editions.

- Martin Kleppmann, *Designing Data-Intensive Applications*.
- Diego Ongaro and John Ousterhout, "In Search of an Understandable Consensus Algorithm" (the Raft paper).
- Martin Kleppmann, "How to do distributed locking" (the Redlock criticism), and Salvatore Sanfilippo's reply.
- Yu. A. Malkov and D. A. Yashunin, the HNSW paper ("Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs").
- Kwon et al., the vLLM / PagedAttention paper.
- Das, Gupta and Motivala, the SWIM membership protocol paper.
- Mohan et al., the ARIES recovery paper.
- Lamping and Veach, "A Fast, Minimal Memory, Consistent Hash Algorithm" (jump hash).
- The Google SRE book chapters on handling overload and avoiding cascading failures.
- Tyler Akidau et al., "The Dataflow Model" paper, and the book *Streaming Systems*.
- Carbone et al., "Lightweight Asynchronous Snapshots for Distributed Dataflows" (Flink checkpointing).
- Martin Fowler, "StranglerFigApplication" and related articles on replacing legacy systems.
- The independent review of the 2018 TSB migration, for a detailed account of a big-bang core migration failure.

---

[← Back to contents](../README.md)
