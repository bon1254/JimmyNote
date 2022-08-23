       Vector3 rot = new Vector3(0, 0, 360f);
       
        WinBigGold_LightBg.transform.DOLocalRotate(rot, 1f, RotateMode.FastBeyond360).SetLoops(20 , LoopType.Incremental).SetEase(Ease.Linear);