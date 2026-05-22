# Evidence Log

Date: 2026-05-22
Workspace: `D:\xbrain\w6\xbrain-costctl-starter`
Fork remote: `https://github.com/DVQuyet/xbrain-costctl-starter.git`
Upstream remote: `https://github.com/TechX-Corp/xbrain-costctl-starter.git`

## Dependency Installation

Command:

```powershell
python -m pip install -r requirements-dev.txt
```

Result: succeeded after network approval. Installed boto3, moto, pytest, and
pytest-cov dependencies for local verification.

## Test Evidence

Command:

```powershell
python -m pytest -v tests
```

Result:

```text
collected 25 items
25 passed in 24.09s
```

Passing areas:

- `tests/test_common.py`
- `tests/test_list.py`
- `tests/test_terminate.py`
- `tests/test_clean.py`

## CLI Help Evidence

Command:

```powershell
python costctl.py --help
```

Result: succeeded. Registered commands:

```text
list, cost, terminate, tag, clean, idle, migrate-gp3
```

## Makefile Evidence

Command:

```powershell
make test
```

Result: blocked in this Windows shell because `make` is not installed.
Equivalent command `python -m pytest -v tests` passed with `25/25`.

## AWS Sample-output Attempts

Real AWS sample outputs could not be captured because the configured AWS
credentials are invalid. The commands were attempted without fabricating output.

Command:

```powershell
python costctl.py list ec2
```

AWS result:

```text
AuthFailure: AWS was not able to validate the provided access credentials
```

Command:

```powershell
python costctl.py cost --tag Application=HealthBot --days 7
```

AWS result:

```text
UnrecognizedClientException: The security token included in the request is invalid.
```

Required follow-up before final submission: refresh AWS credentials, rerun the
read-only sample commands, then replace `sample_output/*_example.txt` with real
account outputs.

## Git Evidence

Current remotes:

```text
origin   https://github.com/DVQuyet/xbrain-costctl-starter.git
upstream https://github.com/TechX-Corp/xbrain-costctl-starter.git
```

Current branch before final documentation commits:

```text
main tracks origin/main
```

There was a stale `.git/index.lock` file present, which must be removed before
creating commits or tags.
