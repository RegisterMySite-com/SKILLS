want to set the same secret for multiple workers? 
Here’s the cleanest way to do exactly that:


```bash
SECRET=$(openssl rand -base64 32)

echo "$SECRET" | npx wrangler secret put TURNSTILE_SECRET --name worker1
echo "$SECRET" | npx wrangler secret put TURNSTILE_SECRET --name worker2
echo "$SECRET" | npx wrangler secret put TURNSTILE_SECRET --name worker3
```

Or as a one-liner loop:

```bash
SECRET=$(openssl rand -base64 32); for w in worker1 worker2 worker3; do echo "$SECRET" | npx wrangler secret put TURNSTILE_SECRET --name "$w"; done
```

Replace `worker1 worker2 worker3` with your actual worker names.