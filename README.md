# React-component-that-renders-a-table-with-pagination

import React, { useState, useEffect } from 'react';

const USERS_URL = 'https://example.com/api/users';

export default function Table () {
const [data, setData] = useState({});
const [page, setPage] = useState(0);
const [isLoading, setIsLoading] = useState(false);

useEffect(() => {
setIsLoading(true);
fetch(`https://example.com/api/users?page=${page}`)
.then(response => response.json())
.then(response => {
setData(response);
setIsLoading(false);
});
}, [page]);

const handleFirstPage = () => {
setPage(0);
};

const handlePreviousPage = () => {
setPage(page - 1);
};

const handleNextPage = () => {
setPage(page + 1);
};

const handleLastPage = () => {
setPage(Math.ceil(data.count / 10) - 1);
};

return (
<div>
<table className="table">
<thead>
<tr>
<th>ID</th>
<th>First Name</th>
<th>Last Name</th>
</tr>
</thead>
<tbody>
{data.results &&
data.results.map(user => (
<tr key={user.id}>
<td>{user.id}</td>
<td>{user.firstName}</td>
<td>{user.lastName}</td>
</tr>
))}
</tbody>
</table>
<div className="pagination">
<button
className="first-page-btn"
onClick={handleFirstPage}
disabled={page === 0 || isLoading}
>
first
</button>
<button
className="previous-page-btn"
onClick={handlePreviousPage}
disabled={page === 0 || isLoading}
>
previous
</button>
<button
className="next-page-btn"
onClick={handleNextPage}
disabled={
page === Math.ceil(data.count / 10) - 1 || isLoading || !data.count
}
>
next
</button>
<button
className="last-page-btn"
onClick={handleLastPage}
disabled={
page === Math.ceil(data.count / 10) - 1 || isLoading || !data.count
}
>
last
</button>
</div>
</div>
);
};
