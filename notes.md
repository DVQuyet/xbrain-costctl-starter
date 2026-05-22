## Nhung phan da implement

### `commands/list_cmd.py`

- Implement list EC2 instances bang `describe_instances` paginator.
- Implement list RDS DB instances va lay tags bang `list_tags_for_resource`.
- Implement list S3 buckets va xu ly bucket khong co tagging config nhu tag rong.
- Implement list EBS volumes bang `describe_volumes` paginator.
- Ho tro filter `--tag key=value` va `--missing-tag key` qua helper `tags_match`.
- Implement output table cho CLI.

### `commands/terminate_cmd.py`

- Implement terminate EC2 bang `terminate_instances`.
- Implement stop RDS bang `stop_db_instance`.
- Implement delete S3 bucket, co guard khong xoa bucket con object.
- Implement delete EBS volume bang `delete_volume`.
- Giu safety contract: confirm y/N mac dinh, `--force` de bypass.
- Bat `ClientError` va in friendly AWS error thay vi traceback.

### `commands/clean_cmd.py`

- Implement `_find_targets(tag_key, tag_val)` cho EC2 va EBS volume.
- EC2: chi target instance chua terminal, bo qua `shutting-down` va `terminated`.
- Volume: chi target volume state `available`, bo qua volume dang attached/in-use.
- Implement dry-run mac dinh.
- Khi co `--apply`, terminate EC2 va delete volume theo plan.

### `commands/cost_cmd.py`

- Implement Cost Explorer `get_cost_and_usage`.
- Filter theo cost allocation tag `--tag key=value`.
- Group theo service dimension.
- Sum `UnblendedCost` theo service va in `TOTAL`.

### `commands/tag_cmd.py`

- Implement convert `--set key=value` thanh boto3 tag list.
- EC2 va volume dung `create_tags`.
- RDS fetch ARN bang `describe_db_instances`, sau do dung `add_tags_to_resource`.
- S3 merge tag moi voi tag hien co truoc khi `put_bucket_tagging`, tranh replace mat tag cu.
- Xu ly S3 bucket chua co tag nhu empty tag set.

### `commands/idle_cmd.py`

- Implement `_avg_cpu` bang CloudWatch `get_metric_statistics`.
- Scan EC2 dang running.
- Bo qua instance co tag `keep=true`.
- In `NO DATA` neu CloudWatch chua co datapoint.
- Danh dau idle neu CPU average nho hon threshold.

### `commands/migrate_gp3_cmd.py`

- Implement dry-run liet ke gp2 volumes va tinh projected monthly savings.
- Ho tro `--apply --volume-id <id>` de migrate mot volume.
- Ho tro `--apply` khong co volume id de migrate tat ca gp2 volumes.
- Dung `modify_volume` voi gp3 baseline: 3000 IOPS, 125 MiB/s.

### `costctl.py`

- Doi help text `gp2 -> gp3` sang ASCII de `python costctl.py --help` chay duoc tren Windows console CP1252.

## Ket qua verify

Da cai dev dependencies bang:

```bash
python -m pip install -r requirements-dev.txt
```

Da chay test:

```bash
python -m pytest -v tests
```

Ket qua:

```text
25 passed
```

Co 1 warning tu pytest cache do Windows khong tao duoc `.pytest_cache`, khong anh huong ket qua test.

Da verify CLI help:

```bash
python costctl.py --help
```

## Trang thai dap ung yeu cau

- Required `list`: da implement.
- It nhat 2 core commands trong `cost`, `terminate`, `tag`: da implement ca 3.
- Stretch commands `clean`, `idle`, `migrate-gp3`: da implement.
- Goal `make test -> 25/25 passing`: dat qua `python -m pytest -v tests`.
- `python costctl.py --help`: chay thanh cong va hien day du commands.
- Fork remote da co: `https://github.com/DVQuyet/xbrain-costctl-starter.git`.
- Upstream remote da co: `https://github.com/TechX-Corp/xbrain-costctl-starter.git`.

## Viec con lai truoc khi nop bai

- Chay CLI voi AWS account that de tao `sample_output` that. Lan verify ngay
  2026-05-22 bi chan vi AWS credentials hien tai invalid:
  - EC2 `AuthFailure`: AWS khong validate duoc access credentials.
  - Cost Explorer `UnrecognizedClientException`: security token invalid.
- Cap nhat README: so nhom, team members, final test score.
- Tao `REFLECTIONS.md` voi it nhat 2 cau tra loi.
- Tao `EVIDENCE.md` ghi lai test, CLI help, remotes, va loi AWS credentials.
- Tao public repo/tag submission:

```bash
git tag w6-sidechallenge-v1
git push --tags
```
