# Performance Analysis Notes

This fork includes annotated comments exploring Ethereum's transaction processing
and how architectural choices impact throughput and latency.

## Files Analyzed

### `core/types/transaction.go`
- Transaction structure and cryptographic overhead
- Signature verification parallelization requirements
- Caching strategies for hash computation
- Cost calculation during mempool admission

## Key Insights

**Signature Verification Bottleneck**
- Each tx requires ~0.5ms ECDSA verification
- At 100k TPS target, requires parallel verification
- Standard Geth: sequential (~2k TPS max)
- High-performance chains: parallel execution model

**State Access Patterns**
- Synchronous state reads are the primary bottleneck
- Disk I/O: ~100μs per random read
- For sub-10ms blocks, state must be in-memory
- Requires aggressive caching and hot state management

**Block Construction Critical Path**
- Transaction selection: priority queue by gas price
- Parallel execution: independent transactions in separate threads
- Conflict detection: optimistic concurrency with rollback
- State commitment: batched Merkle tree updates

## Relevance to Real-Time Blockchains

These annotations explore how next-generation blockchains like MegaETH
achieve 100k+ TPS and sub-10ms latency through:
- Parallel transaction execution
- In-memory state with NVMe backing
- Centralized sequencing for minimal latency
- Optimized commitment to data availability layers

## Author Notes

Created as part of studying Ethereum's architecture and understanding
performance engineering for high-throughput EVM-compatible chains.
