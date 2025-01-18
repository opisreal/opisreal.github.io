<html>
<body>
<!--StartFragment--><h3 data-relingo-block="true">LSM-Tree和B-Tree 对比</h3><h4 data-relingo-block="true"><strong>LSM-Tree 与 B-tree 的存储与修改方式</strong></h4>
<table border="1">
  <thead>
    <tr>
      <th>特性</th>
      <th>LSM-Tree</th>
      <th>B-tree</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>存储结构</td>
      <td>MemTable（内存），SSTable（磁盘）</td>
      <td>B-tree 节点对应磁盘上的页</td>
    </tr>
    <tr>
      <td>数据修改</td>
      <td>顺序写入，新的数据生成新的 SSTable 文件，不直接修改旧文件</td>
      <td>修改数据时直接在对应的页上进行修改</td>
    </tr>
    <tr>
      <td>修改单元</td>
      <td>顺序写入，内存中的 MemTable 或者磁盘中的 SSTable</td>
      <td>修改操作以页为单位，页的大小通常为 4KB、8KB等</td>
    </tr>
    <tr>
      <td>存储优化</td>
      <td>合并操作（Compaction）减少小文件数量</td>
      <td>节点分裂、合并，保持树的平衡</td>
    </tr>
    <tr>
      <td>写入性能</td>
      <td>优化写入，尤其是大量顺序写入</td>
      <td>随机写入性能相对较差，依赖于树的高度</td>
    </tr>
    <tr>
      <td>查询性能</td>
      <td>由于多次磁盘查找，查询性能通常较差，依赖布隆过滤器优化</td>
      <td>查询性能较好，依赖 B-tree 的平衡性</td>
    </tr>
  </tbody>
</table>

<h4 data-relingo-block="true"><strong>LSM-Tree</strong></h4><ul><li><p data-relingo-block="true"><strong>存储结构</strong>：LSM-Tree 使用 <strong>MemTable</strong> 存储内存中的数据，和磁盘上的 <strong>SSTable</strong> 文件。数据的修改不直接在原有数据上进行，而是通过顺序写入的方式更新，新的数据生成新的 SSTable 文件，旧数据不会被直接修改。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3008"><strong>数据修改</strong>：LSM-Tree 的修改主要是顺序写入。内存中的 MemTable 满时会将数据刷新到磁盘，生成新的 SSTable 文件。每次修改并不会修改原来的 SSTable 文件，而是创建新的文件。</p></li><li><p data-relingo-block="true"><strong>修改单元</strong>：数据的修改单元是内存中的 <strong>MemTable</strong> 或者磁盘中的 <strong>SSTable</strong> 文件，而不是直接对页进行操作。</p></li><li><p data-relingo-block="true"><strong>存储优化</strong>：通过 <strong>Compaction</strong> 操作，多个小的 SSTable 文件会被合并成一个更大的文件，从而减少文件数量和优化存储。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3011"><strong>写入性能</strong>：LSM-Tree 优化了顺序写入操作，因此对于大量的写入负载，它表现出色。</p></li><li><p data-relingo-block="true"><strong>查询性能</strong>：由于查询需要在多个 SSTable 文件中查找，并且可能存在多个磁盘访问，查询性能相对较差。但通过 <strong>布隆过滤器</strong> 等优化手段，查询效率得到了提升。</p></li></ul><h4 data-relingo-block="true" data-relin-paragraph="3013"><strong>B-tree</strong></h4><ul><li data-relingo-block="true" data-relin-paragraph="3064"><p><strong>存储结构</strong>：B-tree 将数据存储在 <strong>磁盘页</strong> 中，每个节点（无论是内部节点还是叶子节点）都对应一个磁盘上的页面。B-tree 的节点通常是固定大小，常见的是 4KB 或 8KB 的页面。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3069"><strong>数据修改</strong>：在 B-tree 中，数据的修改是直接在磁盘上的页中进行的。修改操作会影响对应页中的数据，如果页已满，可能会触发节点的 <strong>分裂</strong> 或 <strong>合并</strong> 操作，以保持 B-tree 的平衡性。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3074"><strong>修改单元</strong>：B-tree 的数据修改是 <strong>以页为单位</strong> 进行的，数据被存储在磁盘页内，修改时会操作整个页，页的大小通常为 4KB 或 8KB。</p></li><li><p data-relingo-block="true"><strong>存储优化</strong>：B-tree 通过 <strong>节点分裂</strong> 和 <strong>合并</strong> 操作来保持树的平衡，从而确保查询效率和插入/删除操作的性能。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3083"><strong>写入性能</strong>：B-tree 的写入性能相对较差，特别是在进行大量的随机写入时，因为每次写入可能都需要访问磁盘上的一个或多个页。</p></li><li><p data-relingo-block="true" data-relin-paragraph="3085"><strong>查询性能</strong>：B-tree 在查询时，通过平衡的树结构能够高效地定位数据。查询操作的性能依赖于树的高度，因此其查询效率较高。</p></li></ul><h4 data-relin-paragraph="3088"><strong data-relin-paragraph="3087">总结</strong></h4><ul><li><p data-relingo-block="true"><strong>LSM-Tree</strong> 是通过顺序写入 MemTable 和生成新的 SSTable 来处理数据的修改，而不是直接面向磁盘页修改数据。其写入优化主要依赖于顺序写入和合并操作。</p></li><li><p data-relingo-block="true"><strong>B-tree</strong> 是典型的面向页存储的结构，修改操作通常在页内进行，并且操作是以页为单位执行。B-tree 在保持数据结构平衡的同时，能够提供较高效的查询性能。</p></li></ul><!--EndFragment-->
</body>
</html>