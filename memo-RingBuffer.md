
= RingBuffer の設計メモ

buffer[size] に対するアクセス関数を提供する。

read / last / size の変数を初期化し、buf[size] を確保したと想定。
read は次に読み出す位置、last は次に書き込む位置、size はバッファのサイズ。

    0 <= read < size, 0 <= last < size となるように処理する。
    また、未初期化の場合すべての変数は 0 となると想定する。

    isEmpty()
      read == last の場合に true; それ以外は false;

    isFull()
      ((last + 1) % size) == read の場合に true; それ以外は false;

    isRoomFor(aSize)
      aSize >= size の場合、必ず false;
      read <= last の場合、(last + aSize) < size であれば true; それ以外で ((last + aSize) % size) < read であれば true; それ以外は false;
      last <  read の場合、(last + aSize) < read であれば true; それ以外は false;

    getBufferedSize()
      read <= last の場合、return (last - read);
      last <  read の場合、return ((size - read) + last);