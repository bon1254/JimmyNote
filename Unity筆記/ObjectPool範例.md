
```ad-abstract
title: 原始碼

using UnityEngine.Pool;
using UnityEngine;

public class ObjectPoolTest : MonoBehaviour
{
    private ObjectPool<GameObject> objectPool;

    private void Start()
    {
        // 創建物件池，使用 GameObject 作為對象類型
        objectPool = new ObjectPool<GameObject>(() => CreateCube());
    }

    private GameObject CreateCube()
    {
        GameObject cube = GameObject.CreatePrimitive(PrimitiveType.Cube);
        cube.tag = "Cube"; // 設定標籤為 "Cube"
        return cube;
    }

    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            // 從物件池中獲取對象
            GameObject obj = objectPool.Get();
            if (obj != null)
            {
                obj.transform.position = Random.insideUnitSphere * 5f; // 設置位置
                obj.SetActive(true); // 啟用對象
            }
        }

        if (Input.GetKeyDown(KeyCode.R))
        {
            // 回收對象到物件池
            GameObject[] activeObjects = GameObject.FindGameObjectsWithTag("Cube");
            foreach (GameObject obj in activeObjects)
            {
                objectPool.Release(obj);
                obj.SetActive(false); // 關閉對象
            }
        }
    }
}

```

> [!INFO] ObjectPool Get
> GameObject obj = objectPool.Get();
> 這邊的Get是取下來並生成Cube

> [!INFO] ObjectPool Release
> objectPool.Release(obj);
> 這邊Release是放回去objectpool並關閉Cube