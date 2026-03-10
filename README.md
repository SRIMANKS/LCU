# LeetCode Similar Questions Visualizer

Scrape LeetCode similar questions and build a graph visualization of problem relationships.

## Features

- 🌐 Fetch similar questions from LeetCode GraphQL API
- 🔄 DFS traversal to discover connected problems
- 🧅 **Optional Tor routing** to prevent rate limiting
- 📊 Export data as JSON and text files
- 🌱 Support for single seed or multi-seed scraping

## Quick Start

```bash
# Install dependencies
npm install

# Run with single problem as seed
node script.js two-sum 2

# Run with multiple seeds
node script.js --seeds 2 100

# Enable Tor routing (requires Tor to be running)
node script.js --tor two-sum 2
node script.js --tor --seeds 2 100
```

## Usage

### Command Line Options

```
node script.js [--tor] <problem-slug> [max-depth]
node script.js [--tor] --seeds [max-depth] [seed-limit]
```

**Flags:**
- `--tor` or `-t`: Enable Tor routing (disabled by default)
- `--seeds` or `-s`: Use multi-seed mode

**Parameters:**
- `problem-slug`: Starting problem (default: "two-sum")
- `max-depth`: Maximum DFS depth (default: 2)
- `seed-limit`: Number of seed problems to fetch in multi-seed mode (default: 100)

### Examples

```bash
# Single seed, depth 2, no Tor
node script.js three-sum 2

# Single seed with Tor routing
node script.js --tor three-sum 2

# Multi-seed mode, 100 seeds, depth 2, no Tor
node script.js --seeds 2 100

# Multi-seed mode with Tor routing
node script.js --tor --seeds 2 100
```

## Tor Routing

### Why Use Tor?

Tor routing helps prevent rate limiting by:
- Routing requests through the Tor network
- Automatically rotating IP addresses every 20 requests
- Distributing load across different exit nodes

### Setup Tor

**Option 1: Tor Browser (Easiest)**
1. Download from: https://www.torproject.org/download/
2. Launch Tor Browser and keep it running
3. Tor SOCKS proxy will be available on `localhost:9050`

**Option 2: Standalone Tor Service**

See [TOR_SETUP.md](TOR_SETUP.md) for detailed installation instructions.

### Using Tor

Simply add the `--tor` flag to any command:

```bash
node script.js --tor two-sum 2
```

The script will:
1. ✅ Verify Tor connection on startup
2. 🔄 Automatically rotate IP every 20 requests
3. ⏱️ Rate limit rotations (minimum 10s between changes)

**Note:** Tor is **disabled by default**. You must explicitly enable it with `--tor` flag.

## Output Files

The script generates four files:

- `edges.json` - All problem connections with metadata
- `edges.txt` - Human-readable edge list
- `problems.json` - All unique problems with topics and difficulty
- `problems.txt` - Human-readable problem list

## Rate Limiting

### Without Tor (Default)
- Fixed 1-second delay between requests
- Direct connection to LeetCode API
- May encounter rate limiting with large scrapes

### With Tor (`--tor` flag)
- 1-second delay between requests
- IP rotation every 20 requests
- Reduced rate limiting risk
- Slightly slower due to Tor latency

## Resumable Scraping

The script automatically:
- Loads existing data from previous runs
- Skips already-scraped problems
- Appends new discoveries to existing files
- Preserves data on errors

## Tips

- Start with small depths (1-2) to explore the graph
- Use `--tor` for large multi-seed scrapes
- Monitor output for skipped premium questions
- Resume interrupted scrapes by re-running the same command

## Troubleshooting

### "Cannot connect through Tor"
- Ensure Tor service is running
- Check that SOCKS proxy is on port 9050
- Try running Tor Browser

### Rate Limiting
- Enable Tor: `--tor`
- Increase delay in script.js if needed
- Use smaller seed limits

### Module Not Found
```bash
npm install
```

## License

ISC
