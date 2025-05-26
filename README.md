// Mini App Frontend - React + Tailwind (Telegram Gift Shop)

import { useEffect, useState } from 'react';

export default function TelegramGiftApp() {
  const [user, setUser] = useState(null);
  const [selectedGift, setSelectedGift] = useState(null);
  const [isSubmitted, setIsSubmitted] = useState(false);

  useEffect(() => {
    if (window.Telegram.WebApp) {
      const initDataUnsafe = window.Telegram.WebApp.initDataUnsafe;
      setUser(initDataUnsafe.user);
      window.Telegram.WebApp.expand();
    }
  }, []);

  const gifts = [
    { id: 1, name: 'Плазма', price: 15000, img: 'https://example.com/plasma.png' },
    { id: 2, name: 'Метеор', price: 20000, img: 'https://example.com/meteor.png' },
    { id: 3, name: 'Звезда', price: 17000, img: 'https://example.com/star.png' },
  ];

  const handleBuy = (gift) => {
    setSelectedGift(gift);
    // Optionally send data to bot server (fetch/post)
  };

  const handleSubmit = () => {
    setIsSubmitted(true);
    // send purchase request to server or notify admin
  };

  return (
    <div className="p-4 text-center">
      <h1 className="text-xl font-bold mb-4">Telegram Gift Shop</h1>
      {user && <p className="mb-2">Привет, {user.first_name}!</p>}

      {!selectedGift && (
        <div className="grid grid-cols-1 gap-4">
          {gifts.map((gift) => (
            <div key={gift.id} className="border rounded-2xl p-3 shadow">
              <img src={gift.img} alt={gift.name} className="w-full rounded-xl mb-2" />
              <h2 className="text-lg font-semibold">{gift.name}</h2>
              <p className="text-sm mb-2">Цена: {gift.price.toLocaleString()} сум</p>
              <button
                className="bg-blue-500 text-white px-4 py-2 rounded-xl"
                onClick={() => handleBuy(gift)}
              >
                Купить
              </button>
            </div>
          ))}
        </div>
      )}

      {selectedGift && !isSubmitted && (
        <div className="mt-6">
          <h2 className="text-lg font-bold">Вы выбрали: {selectedGift.name}</h2>
          <p className="mb-2">Отправьте ровно <b>{selectedGift.price + 13}</b> сум на карту: <b>8600 XXXX XXXX XXXX</b></p>
          <p className="mb-4 text-sm text-gray-500">После оплаты нажмите кнопку ниже.</p>
          <button
            className="bg-green-500 text-white px-6 py-2 rounded-xl"
            onClick={handleSubmit}
          >
            Я оплатил(а)
          </button>
        </div>
      )}

      {isSubmitted && (
        <div className="mt-6 text-green-600 font-medium">
          Спасибо! Ваш заказ принят. Мы скоро отправим подарок.
        </div>
      )}
    </div>
  );
}
