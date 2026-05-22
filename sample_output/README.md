# sample_output/

## Current capture status

Read-only account sample captures were attempted on 2026-05-22, but the
configured AWS default profile credentials were invalid:

- EC2 list: `AuthFailure`
- Cost Explorer: `UnrecognizedClientException`
- STS identity check: `InvalidClientTokenId`

The `*_example.txt` files no longer contain the starter's fake resource IDs or
fake costs. They contain the real command attempts and AWS credential errors from
this machine. To complete the final evidence, refresh AWS credentials and rerun
the commands below, replacing each file with successful account output.

See `../EVIDENCE.md` for the captured verification details.

## How to produce real samples

```bash
./costctl.py list ec2 > sample_output/list_ec2_$(date +%F).txt
./costctl.py list ec2 --missing-tag Application > sample_output/list_ec2_missing_app_$(date +%F).txt
./costctl.py cost --tag Application=HealthBot --days 7 > sample_output/cost_HealthBot_$(date +%F).txt
```

The trainer will `git clone` your repo, follow the README, and expect to
reproduce roughly these outputs (allowing for natural drift in timestamps and
resource lists between snapshots).

## Anti-pattern

Don't paste fabricated output. If `costctl list ec2` against your account
returns 0 rows, commit that — it's a valid output. Don't invent fake instance
IDs to make the sample look "interesting".
