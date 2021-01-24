# websocketpp

```
#include <thread>
#include <websocketpp/config/asio_no_tls_client.hpp>
#include <websocketpp/client.hpp>
#include <atomic>
class WebSocktClient
{
    websocketpp::client<websocketpp::config::asio_client> mClient;
    websocketpp::connection_hdl mHdl;
    std::atomic_bool init = false;
    std::unique_ptr<std::thread> mRevThr = nullptr;

    static void on_open(WebSocktClient* obj, websocketpp::connection_hdl hdl) {
        obj->mHdl = hdl;
        obj->init = true;
        std::cout << "suc" << std::endl;
        //websocketpp::client<websocketpp::config::asio_client>::connection_ptr con = obj->mClient.get_con_from_hdl(hdl);
    }
    static  void on_message(WebSocktClient* obj, websocketpp::connection_hdl hdl, websocketpp::config::asio_client::message_type::ptr msg) {
        
        fwrite(msg->get_payload().c_str(), msg->get_payload().size(), 1, stdout);
    }
    static void on_close(WebSocktClient* obj)
    {
        obj->mRevThr = nullptr;
    }
public:
    bool Start(std::string uri = "ws://localhost:8081/con1")
    {
        try {
            mClient.clear_access_channels(websocketpp::log::alevel::all);
            mClient.clear_error_channels(websocketpp::log::elevel::all);
            mClient.init_asio();
            mClient.set_open_handler(websocketpp::lib::bind(&on_open, this, websocketpp::lib::placeholders::_1));
            mClient.set_message_handler(websocketpp::lib::bind(&on_message, this, websocketpp::lib::placeholders::_1, websocketpp::lib::placeholders::_2));
            mClient.set_close_handler(websocketpp::lib::bind(&on_close,this));

            websocketpp::lib::error_code ec;
            websocketpp::client<websocketpp::config::asio_client>::connection_ptr con = mClient.get_connection(uri, ec);
            con->add_subprotocol("sip");
            mClient.connect(con);
            mRevThr = std::make_unique<std::thread>([&]() { mClient.run(); });

            for (int i = 0; i < 10; ++i)
            {
                if (init) return true;
                Sleep(1000);
            }
            return false;
        }
        catch (websocketpp::exception const& e) {
            return false;
        }
        return false;
    }
    void SendStrMsg(std::string msg)
    {
        if (!init) return;
        mClient.send(mHdl, msg.c_str(), websocketpp::frame::opcode::text);
    }
    void SendDat(const char* dat,int len)
    {
        if (!init) return;
        std::error_code ec;
        mClient.send(mHdl, dat, len, websocketpp::frame::opcode::BINARY, ec);
    }
};



int main()
{
    WebSocktClient cl;
    if (!cl.Start()) return 0;
    while (true)
    {
        getchar();
        cl.SendStrMsg("asdfsdf");
    }
}
```