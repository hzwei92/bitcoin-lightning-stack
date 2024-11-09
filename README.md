# Bitcoin Lightning Node Stack

A complete Docker-based stack for running a Bitcoin Lightning node with monitoring capabilities.

## Components
- Bitcoin Core (with optional pruning)
- Lightning Network Daemon (LND)
- Prometheus for metrics collection
- Grafana for visualization
- Bitcoin Exporter for Bitcoin Core metrics

## Prerequisites
- Docker and Docker Compose
- At least 20GB free storage space (500GB+ recommended for full node)
- A VPS or server with at least 2GB RAM
- Open ports capability (or access to firewall settings)

## Quick Start

1. Clone the repository and create required directories:
```bash
git clone <repository-url>
mkdir -p data/{bitcoin,lnd,prometheus,grafana}
```

2. Set up environment variables:
```bash
cp .env.example .env
```

Edit the `.env` file with your preferred settings and strong passwords.

3. Configure your ports:
- Bitcoin Core: 8333 (mainnet)
- LND: 9735 (Lightning peer connections)
- Grafana: 3000 (web interface)

4. Start the stack:
```bash
docker-compose up -d
```
5. Monitor the initial sync:
- Bitcoin Core: Monitor the logs for the sync progress.
- LND: Monitor the logs for the sync progress.

```bash
docker-compose logs -f bitcoind
docker-compose logs -f lnd

```

6. Access Services:
- Grafana: http://your-server-ip:3000 (default login: admin/admin)
- Bitcoin Core RPC: localhost:8332 (via configured credentials)
- LND REST: https://localhost:8080 (requires macaroon authentication)

## Initial Setup

### Create LND Wallet
After initial startup, create your LND wallet:

```bash
docker-compose exec lnd lncli create
```
Follow the prompts to create a new wallet. SAVE YOUR SEED PHRASE SECURELY!

### Configure Grafana
1. Log in to Grafana at http://your-server-ip:3000
2. Go to Configuration → Data Sources
3. Add Prometheus as a data source (url: http://prometheus:9090)
4. Import the provided dashboard JSON from the examples directory (if available)

## Security Considerations
- Change all default passwords in the `.env` file
- Use strong passwords for RPC authentication
- Keep your LND seed phrase secure and backed up
- Consider setting up SSL for Grafana if exposed to the internet
- Use UFW or similar to restrict port access

## Maintenance

### Backing Up
Regular backups are crucial. Back up these files:
- `data/lnd/data/chain/bitcoin/mainnet/channel.backup`
- Your LND seed phrase (stored securely offline)
- `data/bitcoin/wallet.dat` if using Bitcoin Core wallet

### Updating
To update the stack:

```bash
docker-compose pull
docker-compose down
docker-compose up -d
```

### Pruning Bitcoin Core
If using pruning mode, it's configured in `bitcoin/bitcoin.conf`. Default pruning is disabled for full node capability.

## Troubleshooting

### Common Issues
1. **Bitcoin Core won't sync**
   - Check disk space
   - Verify port 8333 is open
   - Check logs: `docker-compose logs bitcoind`

2. **LND won't start**
   - Ensure Bitcoin Core is fully synced
   - Check logs: `docker-compose logs lnd`
   - Verify wallet is created and unlocked

3. **Grafana can't connect to Prometheus**
   - Check Prometheus logs: `docker-compose logs prometheus`
   - Verify Prometheus target configuration

## Support and Contributing
- Report issues via GitHub
- Pull requests welcome
- Join our community discussions

## Project Structure
```
.
├── bitcoin
│   └── bitcoin.conf
├── lnd
│   └── lnd.conf
├── prometheus
│   └── prometheus.yml
├── docker-compose.yml
├── .env.example
├── .env
└── README.md
```

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.