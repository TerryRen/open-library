### Docker log

```bash
journalctl -u docker.service
```

### Docker Container log to temp file

```bash
sudo journalctl CONTAINER_ID=d8f275b0268f --no-pager --since today > x.log
```
