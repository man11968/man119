const hostname = "https://snippets.neib.cn";

export default {
    async fetch(request, env, ctx) {
        // 检查是否为WebSocket升级请求
        if (request.headers.get('Upgrade') !== 'websocket') {
            return new Response('Access Denied: Only WebSocket connections are allowed', { status: 403 });
        }
        
        let url = new URL(request.url);
        // 构建目标URL
        let targetUrl = hostname + url.pathname + url.search;
        
        // 修改请求头指向目标主机
        let newHeaders = new Headers(request.headers);
        newHeaders.set('Host', new URL(hostname).hostname);
        
        // 转发请求
        return fetch(targetUrl, {
            method: request.method,
            headers: newHeaders,
            body: request.body
        });
    }
};