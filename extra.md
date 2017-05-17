# 地图重定向方案

如果需要下载一些不完全和谷歌地图兼容的瓦片地图（切割方式兼容，但是编号规则不同的），可以使用重定向方式，将标准的xyz形式的地址转为实际的链接，在pyMap中访问xyz形式的链接即可。

例如用Jersey进行某地图的重定向为``主路径/tile?x={x}&y={y}&z={z}``的核心代码为：

```java
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.QueryParam;
import javax.ws.rs.core.Response;

/**
 * XX地图重定向服务
 *
 *
 */
@Path("/tile")
public class XXMap {
	@GET
	public Response redirect(@QueryParam("x") int x, @QueryParam("y") int y,@QueryParam("z") int z) throws IOException, URISyntaxException
	{		
		z = z-1;
    y = (int) (Math.pow(2,z+1)+y-1);
    nt n = Math.abs((x + y)) % 4;
		String url = String.format("http://m%d.map.xx.com/hwap?z=%d&x=%d&y=%d&styleid=1000&scene=0&version=227", n,z,x,y);
		URI targetURIForRedirection = new URI(url);
		return Response.seeOther(targetURIForRedirection).build();
	}

}
```

使用``nodejs``或其他方式也可以。这样的好处在于不下载地图也可以转发为标准服务，将来万一修改了升级重定向的规则就可以了。
