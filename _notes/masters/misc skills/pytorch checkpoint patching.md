1. open with `ckpt = torch.load(checkpoint_file, weights_only=False)` in the project root (`mgr2024/`)
2. modify the `state_dict` key
3. save with `torch.save(ckpt, "new_location.ckpt")`